---
title: "Combination of Papers"
date: 2022-08-17
tags: ["others"]
---

There are some papers which is famous and get high number of citation. Always appears when I'm reading current papers. So I decide to write down the most important parts (most concerned by myself, in other way)in the papers

# Learning Deep Features for Discriminative Localization

![image.png](https://s2.loli.net/2022/08/17/xeEUAgGitC461Q7.png)

This is a pretty well-known work from CVPR2016, which has got 6.5k citation numbers up-to-date.

There are 2 parts seems to be important to me: **comparison between global max pooling and global average pooling**, as well as **weakly-supervised object localization**.

## GMP vs. GAP

> We believe that GAP loss encourages the network to identify the extent of the object as compared to GMP which encourages it to identify just one discriminative part.
>
> while GMP achieves similar classification performance as GAP, GAP outperforms GMP for localization.

GAP can focus on a wide range of pixels while GMP only depends on the most significant feature.

## Weakly-supervised Object Localization

They meant weakly-supervised because the labels is image-level but localization is object-level
