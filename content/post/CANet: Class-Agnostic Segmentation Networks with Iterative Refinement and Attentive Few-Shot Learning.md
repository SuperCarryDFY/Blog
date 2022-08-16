---
title: CANet: Class-Agnostic Segmentation Networks with Iterative Refinement and Attentive Few-Shot Learning
date: 2022-08-16
tags: ["Few-Shot Learning","segmentation"]
---

![image.png](https://s2.loli.net/2022/08/16/AiIQyMX2cZBlStK.png)

从TPAMI20那篇过来的，主要想看一下哪里说的middle-level feature

## ABSTRACT & INTRODUCTION

![image.png](https://s2.loli.net/2022/08/16/9DYVUmMyPNJnwkq.png)

consists of 

- a two-branch dense comparison module
  - performs multi-level feature comparison between the support image and the query image
- an iterative optimization module 
  - iteratively refines the predicted results. 

fancy的地方

- 先前的工作，从1-shot拓展到k-shot时，都是用non-learnable fusion，这篇文章中用的是attention mechanism。
- 做test的时候，不再输入support image mask了，而是输入support image bounding box.

## METHOD

![image.png](https://s2.loli.net/2022/08/16/Ea6tJYghbZvXCiW.png)

### DENSE COMPARISON MODULE

在CNN中

- feature in low layers often relate to low-level cues 比如颜色，边缘。个人感觉是因为感受野不够大
- feature in higher layers relate to object-level concepts 比如物品种类

> 因为他们要求模型训练完之后具备一定的generalization，而middle-level fature有可能会包含来自没见过的物体的part。比如说训练的时候见过小轿车，那么在测试中，我就比较容易根据middle-level中的轮胎来segment公交车。（make sense）

**dialated convolution**空洞卷积

以下来自[知乎](https://www.zhihu.com/question/54149221)

>  7 x 7 的卷积层的正则等效于 3 个 3 x 3 的卷积层的叠加。而这样的设计不仅可以大幅度的减少参数，其本身带有正则性质的 convolution map 能够更容易学一个 generlisable, expressive feature space。这也是现在绝大部分基于卷积的深层网络都在用小卷积核的原因。
>
> 传统卷积核的本质问题：pool减小图片尺寸，upsampling增大图片尺寸，在这个过程中肯定有信息丢失掉了。因此空洞卷积就是不通过pooling也能获得较大感受野的方法
>
> dialated convolution的优点：内部数据结构的保留和避免使用 down-sampling 。

具体的，他们把Resnet分成4个block，只用block2和block3的输出，将其concat到一起。后面就是support feature与support feaure相乘来把背景像素清0，avg pool之后repeat到之前的维度和query feature concat到一起，比较常规。

### INERATIVE OPTIMIZATION MODULE



