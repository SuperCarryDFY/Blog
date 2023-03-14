---
title: "Affordance Transfer Learning for Human-Object Interaction Detection"
date: 2022-08-22
tags: ["Human-Object Interaction","Affordance"]
categories : ["CV"]
license: CC BY-NC-ND
image: "https://s2.loli.net/2022/08/22/9m3qWEVyUQIHfgB.png"
---

![image.png](https://s2.loli.net/2022/08/22/avbpq9mtxy5IdCF.png)

## ABSTRACT & INTRODUCTION

这篇实质是完成一个分类任务，并且能在unseen objects上也能辨认出其affordance，应该比较容易拿过来做few-shot任务。

![image.png](https://s2.loli.net/2022/08/22/UltqHo4P2MOs9Lf.png)

## METHOD

train用这张图，test用下面object affordance recognition那张图

![image.png](https://s2.loli.net/2022/08/22/9m3qWEVyUQIHfgB.png)

### AFFORDANCE TRANSFER LEARNING

**Efficient HOI Composition**

To compose a new HOI by the object $\hat{l_o}$ and verb $l_v$, we assign the label to the composite HOI as follows,

$$
\hat{y} = (\hat{l_o}A_o) \and (l_vA_v)
$$

，其中$A_o$和$A_v$是分别关于object和verb的同现矩阵co-occurrence matrix（？）

**Invalid HOI Elimination**

有些HOI是无效的（比如ride orange），所以把无效HOI在上式矩阵的对应位置清零了

### OBJECT AFFORDANCE RECOGNITION

这里主要解释如何在test phase做推理

![image.png](https://s2.loli.net/2022/08/22/CaRPrfTUmpjAqtX.png)

对每个affordance，我们随机抽取M（这里M=100）个instances，抽取完特征之后作为affordance feature bank

对一个输入的object feature，我们把它和bank所有的affordance一一结合起来，把所有的HOI predictions都转换成affordance prediction（这有啥区别），然后就得到了有许多重复元素的affordance lists。一个元素重复得越多说明有这个affordance的可能习惯越大。

### OPTIMIZATION AND INFERENCE

loss就比较常规
$$
L = L_{hoi\_sp}+\lambda_1L_{hoi}+\lambda_2L_{ATL}
$$
，其中$\lambda_1$,$\lambda_2$都是超参数。