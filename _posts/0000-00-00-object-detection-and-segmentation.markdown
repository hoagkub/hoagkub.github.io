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

