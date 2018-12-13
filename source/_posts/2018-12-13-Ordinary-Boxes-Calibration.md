---
title: Ordinary Boxes Calibration
date: 2018-12-13 19:56:23
tags:
description: Paper Review - 2018 - Accurate Calibration of LiDAR-Camera Systems using Ordinary Boxes
categories:
- LiDAR-CAMERA Calibration Papers
---

## Calibration using Ordinary Boxes

2018 - Accurate Calibration of LiDAR-Camera Systems using Ordinary Boxes

#### Key Idea

Using ordinary boxes and the boxes are cheap and easy to manufacture.

The performance of the boxes is good, easy to determine the edges of boxes.

low resolution but good performance

**Requirement**: 3 sides of the boxes should be scanned

##### Steps:

1. Plane Detection: RANSAC algorithm to chop rough area

2. Outliner Removal: RANSAC

3. Iterative Box Refinement

   1. Rotation: Minimizing the sum of square errors
   2. Translation

4. Convergence

5. Calculation:

   Corner points can be calculated

#### Why Multi Sensor Calibration

The extrinsic calibration is necessary for these sensors to work together, which means that their relative position and orientation need to be known a priori.

[^1]: Accurate Calibration of LiDAR-Camera Systems using Ordinary Boxes

#### Existing Methods on LiDAR Pair Calibration

1. Using planar chessboards
2. Using different form of planar surfaces
3. Using no calibration objects

The downside of using planar surfaces is that accurate detection of object's edges is hard in the LiDAR point cloud.

**The pattern of checkboard will pollute the LiDAR point clouds**