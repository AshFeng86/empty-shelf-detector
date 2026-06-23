# Empty-shelf-detector

Fine-tuned YOLO11n to detect out-of-stock regions on retail shelves.

## What it does
Detect empty/out-of-stock areas in shelf photos and reports the number of gaps and the empty shelf % area - a proxy for on-shelf availability, the metric brands track because stockouts directly leak sales.

## Results
- Dataset: Roboflow "Supermarket Empty Shelf Detector" (~500 images, single class)
- Model: YOLO11n, fine-tuned 50 epochs on Colab T4
- v1 (Claude version): mAP50: 0.884 | mAP50-95: 0.615
- v2 (Redo version): mAP50: 0.884 | mAP50-95: 0.615
The redo version has the same output as AI (Claude) version. However, the empty shelf detection rate drops significantly while encountering my retail images. (only 2 out of 7 images have found empty shelves, not to mention many empty shelves are not being labeled).

## Real-world test
Tested on photos I took at a local store. Detection rate dropped noticeably versus the benchmark - a clear case of domain shift: the model generalizes poorly to shelves that don't resemble its training data. Next step is fine-tuning on domain-matched (locally captured) images.

## Caveat
- The single class model is apparently not enough for brands, the current version only contains a single class - empty shelf, which means gaps only. Therefore, it's constrained and cannot perform share-of-shelf calculation
- The empty-area % double-counts overlapping boxes, so it slightly overstates how much shelf is empty. A two-class dataset (product+gap) would fix both this and the share-of-shelf limitation.

## Stack
Python, Ultralytics YOLO11, Google Colab, Roboflow
