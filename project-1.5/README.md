# Project 1.5 Empty Shelf Detector
Fine-tune YOLO11n to detect out-of-stock regions on retail shelves.

## What it does
Retailers lose significant revenue annually on empty shelves. This AI reads images to detect empty shelves automatically, so more retailers will not miss their chances.

## Results on Taiwanese retailers (Pmax, Cosmed, 7-11, animal shop, etc)
- Validation set: mAP50: 0.578 | mAP50-95: 0.251
- Test set: mAP50: 0.451 | mAP50-95: 0.194 | Recall: 0.348 | Precision: 0.778

## Caveat
High precision, low recall indicates the model rarely shoots, but once it opens fire, it hits the right target. This could be caused by a lack of training images. The current version only contains 71 testing images.

## Learning
The result (high precision, low recall) is the opposite of what a retailer wants; we'd rather have a false alarm (empty gap detected, but it's actually not empty) rather than missing some gaps. The goal should be to tune the recall rate up by sacrificing a fraction of precision. This could be done immediately by lowering the confidence threshold at inference (recall up, precision down, same model), and more fundamentally by collecting more training data (300-500+ images) to raise the whole precision-recall curve.
