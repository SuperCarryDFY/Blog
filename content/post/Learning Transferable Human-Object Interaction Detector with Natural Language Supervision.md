---
title: "Learning Transferable Human-Object Interaction Detector with Natural
Language Supervision"
date: 2022-08-22
tags: ["Human-Object Interaction"]
categories : ["CV"]
license: CC BY-NC-ND
image: "https://s2.loli.net/2022/08/22/d7nPavWht2iRBue.png"
---

![image-20220822150720449](https://s2.loli.net/2022/08/22/gWoakEicOMpR68j.png)

## ABSTRACT & INTRODUCTION

这篇文章的工作是，输入一张图片，输出HOI特征，并且用HOI短语作为监督训练（基于CLIP）。

他与其他HOI transfer工作不同的点在于，之前的工作对unseen object都是用discrete label作为输出，得到HOI。但是这就要求label中对应的HOI（就是每个动作）都预训练过，很难去识别interaction that out of the predefined list。这篇的思路主要是对文字和图像同时encode，然后寻找最近的匹配对，所以不存在这个问题。

![image-20220822152554566](https://s2.loli.net/2022/08/22/IGi64rQnLkNBma3.png)

## METHOD

![image-20220822152833390](https://s2.loli.net/2022/08/22/d7nPavWht2iRBue.png)

他们定义了HOI为 $\{(b_p,b_o,a,o)\}$（有些文章定义为triplet，其实都差不多，最重要的是verb），其中$b_p$和$b_o$是人和物的bounding box。$a$和$o$分别是human action和object category。

### PRELIMINARY

在preliminary中，他们简单设想了一些步骤，用Faster RCNN得到bounding box然后输入到CLIP，就能在Unseen数据集上得到SOTA，even without tuning.（废话，人家设计的时候也没管unseen HOI啊）

![image-20220822155533112](https://s2.loli.net/2022/08/22/36FEm9MqlODH4wT.png)

### PROPOSED METHOD

- visual input: $\{(h,c,b_p,b_o)\}$, 
  - h is feature representation for interactions
  - $b_p,b_o$ is bounding box
  - c is the confidence score for bounding box prediction
- text encoder: raw text of interactions

- similarity $h^Ts$, where h, s denote the output of visual encoder and text encoder. They are semantic features in the same dimension.

