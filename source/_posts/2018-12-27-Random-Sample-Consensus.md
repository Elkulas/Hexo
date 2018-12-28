---
title: Random Sample Consensus
date: 2018-12-27 20:56:53
tags:
mathjax: true
categories: 
- Camera Calibration
description: This article mainly illustrates the main idea of random sample consensus (RANSAC) approach. 
---

## Random Sample Consensus (RANSAC)

### Background

For a series of data set, if we want to figure out the inside model or the relationship of the data, least-squares estimation approach can be adopted. However, what if the data is contaminated? 

The figure below shows the problem. 

<img src="2018-12-27-Random-Sample-Consensus\plot_ransac_1.png" style="width:300px height:300px">

From our perspective, we can clearly distinguish the line with the highest probability to represent the data set, which is the blue one. However not the LSE. LSE will be greatly disrupted by the noise points and other points which are not matter at all. In our example these points are the points located on the down-left. A **robust** way to select points and build the model is to use RANSAC.

Before going further on the algorithm explanation and analysis, we need to introduce two terms for the data. "Inliers" represent data points whose distribution can be explained by some set of model parameters. "Outliers" represent data points which are not fit the model, generated from erroneous measurements or incorrect hypotheses about the interpretation of data. 

Also, RANSAC assumes that given a set of liners, there exists a procedure which can estimate the parameters of a model that optimally explains or fits this data. The main idea for the  RANSAC is to find the optimal explanation of the model.

View estimation as a two-stage process:

- Classify data points as outliers or inliers
- Fit model to inliers while ignoring outliers



### Overview

The RANSAC algorithm is essentially composed of two steps that are iteratively repeated:

1. In the first step, a sample subset containing minimal data items is randomly selected from the input dataset (This means, if the output relation is a line, the minimal items is 2, if the relation is the transformation matrix, the min should be 3 pairs of points).  A fitting model and the corresponding model parameters are computed using only the elements of this sample subset. The cardinality of the sample subset is the smallest sufficient to determine the model parameters.
2. In the second step, the algorithm checks which elements of the entire dataset are consistent with the model instantiated by the estimated model parameters obtained from the first step. A data element will be considered as an outlier if it does not fit the fitting model instantiated by the set of estimated model parameters within some error threshold that defines the maximum deviation attributable to the effect of noise.

The set of inliers obtained for the fitting model is called consensus set. The RANSAC algorithm will iteratively repeat the above two steps until the obtained consensus set in certain iteration has enough inliers.[^1]

The following graphs well illustrate the principle of the two steps shown above. 

##### Original data set

<img src="2018-12-27-Random-Sample-Consensus\1.png" style="width:300px height:200px">

##### First sample

<img src="2018-12-27-Random-Sample-Consensus\2.png" style="width:300px height:200px">



<img src="2018-12-27-Random-Sample-Consensus\3.png" style="width:300px height:200px">

##### Second sample

<img src="2018-12-27-Random-Sample-Consensus\4.png" style="width:300px height:200px">



<img src="2018-12-27-Random-Sample-Consensus\5.png" style="width:300px height:200px">

##### Third sample

<img src="2018-12-27-Random-Sample-Consensus\6.png" style="width:300px height:200px">



<img src="2018-12-27-Random-Sample-Consensus\7.png" style="width:300px height:200px">

##### Fourth sample

<img src="2018-12-27-Random-Sample-Consensus\8.png" style="width:300px height:200px">



<img src="2018-12-27-Random-Sample-Consensus\9.png" style="width:300px height:200px">

##### Optimal selection

<img src="2018-12-27-Random-Sample-Consensus\10.png" style="width:300px height:200px">

After the 4 iterations, the parameter of the model is determined. Literally, the more iteration times is adopted, the more accurate the model will be to represent the data set. However, there DO exist some tricks to set the max iteration number.

The algorithm can be explained as below.[^ 2]

[^]: 

<img src="2018-12-27-Random-Sample-Consensus\11.png" style="width:600px height:400px">

### Parameters

#### Parameter N

The key point is to choose the proper parameters. The parameters required is shown below:

- e = probability that a point is an outlier
- s = number of points in a sample
- N = number of samples (we want to compute this)
- p = desired probability that we get a good sample

The parameter $e$ and $s$ are given by the data set itself, $p$ depends on our requirement. The only parameter we have to design is $N$. $N$ meets the equation below:

$$1-(1-(1-e)^{s})^{N} = p$$

where,

$1-e$ denotes the probability that choosing one point yields an inlier;

$(1-e)^{s}$ denotes the probability of choosing $s$ inliers in a row (sample only contains inliers);

$1-(1-e)^{s}$ denotes the probability that one or more points in the sample were outliers (sample is contaminated);

$(1-(1-e)^{s})^{N}$ denotes the probability that $N$ samples were contaminated;

$1-(1-(1-e)^{s})^{N} $ denotes the probability that at least one sample was not contaminated (at least one sample of $s$ points is composed of only inliers). [^2]

if  $p$  is determined, the desired $N$ is determined by the following equation.

$$N = \frac{\log(1-p)}{\log(1-(1-e)^{s})}$$

#### Example of N

Right the example shown below

$n=12$ points is in the data set, the minimal sample size $s = 2$, outliers ratio $e = 1/6 => 20\%$

So the $N = 5$ give us a 99% chance of getting a pure-inlier sample.

![](2018-12-27-Random-Sample-Consensus\Snipaste_2018-12-28_16-53-07.png)

#### Parameter d

Another important parameter is distance threshold $d$. Usually $d$ is chosen empirically. but when  measurement error is known to be Gaussian with mean 0 and variance $S^2$, there are ways to determine $p$. 



[^1]: https://en.wikipedia.org/wiki/Random_sample_consensus
[^2]: http://www.cse.psu.edu/~rtc12/CSE486/lecture15.pdf

