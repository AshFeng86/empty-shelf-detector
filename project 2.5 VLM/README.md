# Empty Shelf Detector
Compare a fine-tuned YOLO11n detector against Claude Sonnet 4.6 (VLM) on the same 10 Taiwanese retail shelf images to find which role each should play in a shelf-analytics system. The detector locates the gaps; the VLM is tested on whether it can reason about shelf stock from natural-language prompts.

## Results
img    YOLO  VLM-p1  VLM-p2
[0]    6       5       0
[1]    5       5       0
[2]    4       4       0
[3]    3       5       0
[4]    1       4       0
[5]    1       3       0
[6]    2       4       0
[7]    5       4       0
[8]    1       4       0
[9]    4       3       1

Detector: mAP50 0.451 | recall 0.348 | 12.1ms/img
VLM tokens/10: p1 9837 | p2 7858

## Findings
- **VLM gap counts are driven by the prompt, not the image.** p1 ("analyze this shelf for visible stock gaps") pushes the model to find gaps - it returns ~4 on every image regardless of content. p2 ("Retail shelves are USUALLY fully stocked") makes it conservative - it returns 0 on almost all. Same 10 images, opposite results. The framing dominates the pixels.
- **VLM output also varied run-to-run** at default temperature (same image gave different counts on repeat calls). Set 'temperature = 0' for a reproducible comparison
- **The detector responds to the image** - counts vary per shelf (6 -> 1 )and are identiacl every run. But recall is only 0.348: it misses ~2/3 of real gaps.
  
## Takeaway
Neither tool counts gaps well on its own, but they fail differently, which points to a two-role split:
- **Detector** = *where / how many* - reliable, reproducible, 12ms (real-time, on-robot), but low-recall
- **VLM** = *what it is* - reads brands and describes sections in language (cloud-side analytics), but should **not** be trusted to count or locate gaps.
