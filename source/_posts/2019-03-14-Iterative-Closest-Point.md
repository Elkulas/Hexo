---
title: Iterative Closest Point
date: 2019-03-14 20:23:55
tags:
mathjax: true
categories:
- Camera Calibration
description: This article briefly discusses the main idea of ICP alogorithm for the reminding work
---

## Iterative Closest Point

### Description

Given the following point sets:
$$X =\{ x_1,...,x_{N_x}\}$$

$$P =\{ p_1,...,p_{N_p}\}$$

**Wanted:**

Translation $t$ and rotation $R$ that minimize the sum of the squared error

$$E(R,t) = \frac{1}{N_p}\sum{}$$