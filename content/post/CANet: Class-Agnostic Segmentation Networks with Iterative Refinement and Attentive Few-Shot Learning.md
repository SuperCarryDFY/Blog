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
