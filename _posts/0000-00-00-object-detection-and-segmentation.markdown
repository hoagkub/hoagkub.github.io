---
layout: post
title:  "Object Detection and Segmentation"
date: "2024-08-28 20:00:00 +0700"
last_modified_at: "2024-08-28 20:00:00 +0700"
categories: [Study, Deep_Learning]
---

![Image not found](/assets/img/object-detection-and-segmentation/ink_1.png)
![Image not found](/assets/img/object-detection-and-segmentation/ink_2.png)

# VI. Objective Detection Metric

## VI.1. (Recall) Precision & Recall

![Image not found](/assets/img/object-detection-and-segmentation/ink_3.png)
_Source: <https://en.wikipedia.org/wiki/Precision_and_recall>_

- True Positives: Classified as Positive and that was indeed Positive
- True Negatives: Classified as Negative and that was indeed Negative
- False Positives: Classified as Positive and that was indeed Negative
- False Negatives: Classified as Negative and that was indeed Positive

## VI.2. IoU - Intersection over Union

![Image not found](/assets/img/object-detection-and-segmentation/ink_4.png)
_Source: Udacity Deep Learning Nanodegree Program_

> IoU = Intersection / Union

- Intersection is the intersection between Bounding Box and Ground Truth
- Union is the union between Bounding Box and Ground Truth

## VI.3. mAP (Mean Average Precision) & mAR (Mean Average Recall)

### VI.3.1 mAP (Mean Average Precision)

\[ mAP = {\sum_{i=1}^K AP_i \over K} \]

\(AP_i\) is computed as follows

![Image not found](/assets/img/object-detection-and-segmentation/prec-recall-curve.gif)
_Source: Udacity Deep Learning Nanodegree Program_

![Image not found](/assets/img/object-detection-and-segmentation/prec-recall-interp.gif)
_Source: Udacity Deep Learning Nanodegree Program_

### VI.3.2 mAR (Mean Average Recall)

\[ mAR = {\sum_{i=1}^K AR_i \over K} \]

\(AR_i\) is computed as follows

![Image not found](/assets/img/object-detection-and-segmentation/iou-recall-curve.gif)
_Source: Udacity Deep Learning Nanodegree Program_

