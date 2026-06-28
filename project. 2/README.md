# Real-Time Video Empty Shelf Detector

Runs a YOLO11n model (fine-tuned on self-collected Taiwanese retail store
photos) on video, with object tracking via Ultralytics `model.track()`, to
detect empty shelf gaps frame-by-frame.

## Results
- At conf=0.1, the model reliably detects empty gaps when clearly visible,
  and holds a stable track ID on a gap that stays in frame (verified: gap #1
  kept ID:1 across the clip).
- Detects partial gaps too — a slot doesn't have to be fully sold out for the
  model to flag the emptiness.

## Limitations (two separate problems)
1. **Detection quality — a data problem.** Boxes flicker frame-to-frame, and
   3 adjacent gaps are inconsistently detected as 2 or 3 boxes. Root cause is
   the training-data ceiling (test mAP50 = 0.45; note YOLO11n is the smallest
   model variant). More well-annotated retail data would raise this.
2. **ID persistence across re-entry — a tracker problem.** When a gap leaves
   frame and returns, it gets a new ID; with BoT-SORT, IDs climbed into the
   hundreds. Re-ID matches objects by appearance, but empty gaps are
   near-identical low-texture rectangles with no distinctive features to match
   on — so re-ID can't re-link them. More data does *not* fix this.

## Takeaway
These are different failures on different axes: detection quality scales with
data, ID persistence does not. For the end goal (locate gaps and restock), the
robot acts on the current frame, so persistent cross-frame IDs aren't on the
critical path yet — re-ID is deferred to a later analytics phase.

