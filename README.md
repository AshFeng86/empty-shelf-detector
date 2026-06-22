# Empty-shelf-detector

Fine-tuned YOLO11n to detect out-of-stock regions on retail shelves.

## What it does
Detect empty/out-of-stock areas in shelf photos and reports the number of gaps and the share of shelf area that's empty - a proxy for on-shelf availability, the metric brands track because stockouts directly leak sales.

## Results
- Dataset: Roboflow "Supermarket Empty Shelf Detector" (~500 images, single class)
- Model: YOLO11n, fine-tuned 50 epochs on Colab T4
- mAP50: 0.884 | mAP50-95: 0.615

## Real-world test
Tested on photos I took at a local store. Detection rate dropped noticeably versus the benchmark - a clear case of domain shift: the model generalizes poorly to shelves that don't resemble its training data. Next step is fine-tuning on domain-matched (locally captured) images.

## Stack
Python, Ultralytics YOLO11, Google Colab, Roboflow
