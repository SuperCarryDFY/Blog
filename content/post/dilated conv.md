---
title: "Dilated Convolution & Deconvolution "
date: 2022-10-13
tags: ["Others"]
categories : ["CV"]
license: CC BY-NC-ND
---

## Dilated Convolution

Common Conv

![动图](https://pic2.zhimg.com/50/v2-d552433faa8363df84c53b905443a556_720w.webp?source=1940ef5c)

Dilated Conv

![image-20221023221652075](https://github.com/vdumoulin/conv_arithmetic/blob/master/gif/same_padding_no_strides.gif?raw=true)

空洞卷积在一定程度上能增大卷积神经网络的感受野，但是利用其设计语义分割网络则会存在如下两个问题。

**1. The Gridding Effect	**

如果和之前的操作一样，仅仅只是反复叠加3*3的kernal的话，那么在过程中就会存在一定的信息损失。

![img](https://pic3.zhimg.com/v2-478a6b82e1508a147712af63d6472d9a_r.jpg?source=1940ef5c)

**2. 对小物体的分割**

增大感受野对小物体的分割似乎没有好处

因此图森组提出了较好的解决方法：Hybrid Dilated Convolution (HDC)

1. 叠加卷积的 dilation rate 不能有大于1的公约数。比如 [2, 4, 6] 则不是一个好的三层卷积，依然会出现 gridding effect。我们将 dilation rate 设计成锯齿状结构，例如 [1, 2, 5, 1, 2, 5] 循环结构。
2. 需要满足$$M_i = max[M_{i+1}-2r_i, M_{i+1}-2(M_{i+1}-r_i), r_i]$$， 其中$r_i$，$M_i$分别代表第i层的dilation rate和最大dilation rate。

来自[如何理解空洞卷积（dilated convolution）？](https://www.zhihu.com/question/54149221)， 作者@[刘诗昆](https://www.zhihu.com/people/lorenmt)



## Deconvolution

deconv大致可以分成如下三个方面

- unsupervised learning
- CNN可视化
- upsampling

呃。。一般来说上采样+卷积的性能比反卷积要好，而且反卷积存在棋盘格效应。

来自[如何理解深度学习中的deconvolution networks？](https://www.zhihu.com/question/43609045/answer/132235276)， 作者@[谭旭](https://www.zhihu.com/people/xutan)

