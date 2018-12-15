---
title: Perspective-n-points Problem
date: 2018-12-15 11:49:59
tags:
mathjax: true
categories:
- Camera Calibration
description: This article briefly discusses the main idea of PnP problem and set up the question model of P3P problem. No solutions are given.
---

## Perspective n Points Problem - P3P

### Problem Definition

Given a set of *n* 3D points in a world reference frame and their corresponding 2D image projections as well as the calibrated intrinsic camera parameters, determine the 6 DOF pose of the camera in the form of its rotation and translation with respect to the world. This follows the perspective project model for cameras:

$$s\ p_c = K[R|T]p_w$$

![](2018-12-15-Perspective-n-points-Problem\Snipaste_2018-12-15_11-53-29.png)

$p_w=[x\ y\ z\ 1]^T$is the homogeneous world point, $p_c = [u\ v\ 1]^T$is the corresponding homogeneous image point. $K$ denotes the intrinsic parameters matrix, where $\gamma$ is the skew parameter. $s$ is a scale factor for the image point. and $R$ and $T$ are the desired 3D rotation and 3D translation of the camera (extrinsic parameters) that are being calculated.

### Aassumptions

The key assumption is that the intrinsic matrix $K$ should be given. For each solution to PnP, the chosen point correspondences cannot be coplanar. 

### Method Description

​	When n = 3, the PnP problem is in the minimal form of P3P and can be solved with three point correspondences. Let $P$ be the Center of Perspective, and $A,B,C$ the control points. Let$|PA|=X,\ |PB| = Y,\ |PC|=Z$, $\alpha = \angle BPC,\ \beta = \angle APC, \ \gamma = \angle APB$ , $p = 2\cos \alpha,\ q = 2\cos \beta, \ r = \cos \gamma$ , $|AB|=c',\ |BC| = a',|AC| = b'$.From triangles $PBC,PAC,PAB$ ，we obtain the equation system.

![](2018-12-15-Perspective-n-points-Problem\Snipaste_2018-12-15_13-39-21.png)

​	A set of solutions for $X,Y,Z$ is call a set of *physical solutions* if the following "reality conditions" are satisfied. These conditions are assumed through out the article.

![](2018-12-15-Perspective-n-points-Problem\Snipaste_2018-12-15_13-53-18.png)

To simplify the question, let $X = xZ,Y = yZ,|AB| = \sqrt{v}Z$ , $|BC| = \sqrt{av}Z$ , $|AC| = \sqrt{bv}Z$. Since $Z = |PC| \neq 0 $, we obtain the following equivalent equation system:

![](2018-12-15-Perspective-n-points-Problem\Snipaste_2018-12-15_14-00-53.png)

Since $|r|<2$, we have $v = x^2+y^2-xyr>0$. Thus $Z$ can be uniquely determined by $Z = |AB|/\sqrt{v}$. Eliminating $v$ from the equations above, we have ![](2018-12-15-Perspective-n-points-Problem\Snipaste_2018-12-15_14-09-52.png)

Now the P3P problem is reduced to finding the positive solutions of two quadratic equations. As a consequence, we obtain the following result:

**The P3P problem has either an infinite number of solutions  or at most four physical solutions**

The following solutions to these two equations are not given in this article, please refer to the paper.[^1]





[^1]: Complete solution classification for the perspective-three-point problem, Gao, Xiao ShanHou, Xiao RongTang, JianliangCheng, Hang Fei







