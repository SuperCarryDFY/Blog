---
title: "Combination of Papers"
date: 2022-08-17
tags: ["others"]
---

There are some papers which is famous and have gotten high number of citation. Always appears when I'm reading current papers. So I decide to write down the most important parts (mostly concerned by myself, in other way)in the papers.

# Learning Deep Features for Discriminative Localization

![image.png](https://s2.loli.net/2022/08/17/xeEUAgGitC461Q7.png)

This is a pretty well-known work from CVPR2016, which has got 6.5k citation numbers up-to-date.

There are 2 parts seems to be important to me: **comparison between global max pooling and global average pooling**, as well as **framework**.

## GMP vs. GAP

> We believe that GAP loss encourages the network to identify the extent of the object as compared to GMP which encourages it to identify just one discriminative part.
>
> while GMP achieves similar classification performance as GAP, GAP outperforms GMP for localization.

GAP can focus on a wide range of pixels while GMP only depends on the most significant feature.

## FRAMEWORK

![image-20220817142844844](/home/dai/snap/typora/57/.config/Typora/typora-user-images/image-20220817142844844.png)

The output of the last convolutional layer is denoted as $f_k(x,y)$, while k means the channel. After GAP, which should be expressed as $F^k = \sum_{x,y}f_k(x,y)$, for a given class, the input to the softmax $S_c$, is $\Sigma_kw_k^cF_k$ where $w_k^c$ is weight corresponding to class c for unit k. 
$$
S_c = \sum_kw_k^c\sum_{x,y} f_k(x,y) = \sum_{x,y}\sum_k w_k^cf_k(x,y)
$$
They did upsampling in the middle of the framework to fit the size.

ABOUT **weakly-supervised**: They meant weakly-supervised because the labels is image-level but localization is object-level
