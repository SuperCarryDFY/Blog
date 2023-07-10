---
title: "SCORE-BASED GENERATIVE MODELING THROUGH STOCHASTIC DIFFERENTIAL EQUATIONS"
date: 2023-07-04
license: CC BY-NC-ND
tags: ["Diffusion Models"]
categories : ["CV"]
image: "SCORE-BASED%20GENERATIVE%20MODELING%20THROUGH%20STOCHASTIC%2091e1e486eac24bc0b30b85fedb4f4862/Untitled.png"
---



# SCORE-BASED GENERATIVE MODELING THROUGH STOCHASTIC DIFFERENTIAL EQUATIONS

## Score based generative modeling with SDEs

diffusion process可以被表述为以下形式 

![Untitled](Untitled.png)

reverse-time SDE可以被表述为以下形式（可以看到需要知道分布分数s_theta）

![Untitled](Untitled%201.png)

### estimating scores for the SDE

和SMLD那篇文章一样，用denoising score matching的方式训练：

![Untitled](Untitled%202.png)

### VE,VP SDEs and Beyond

这里讲了SMLD,DDPM和SDE的关系（SMLD和DDPM可以看作离散的SDEs的两种不同模式）。

对SMLD（Variance Exploding SDE）：

![Untitled](Untitled%203.png)

对DDPM（Variance Preserving SDE）：

![Untitled](Untitled%204.png)

## Solving the reverse SDE

作者提出了三种采样方式

### general-purpose numerical SDE solvers（reverse diffusion samplers）

DDPM的采样方法

![Untitled](Untitled%205.png)

被称之为祖先采样（ancestral sampling），而作者提出了reverse diffusion samplers

![Untitled](Untitled%206.png)

可以证明，ancestral sampling，当beta_i趋近于0的时候，可以转化为reverse diffusion samplers的形式

### Predictor-corrector samplers

![Untitled](Untitled%207.png)

### probability flow

对于每个SDE，存在一个确定性的diffusion过程：ODE

![Untitled](Untitled%208.png)

ODE速度更快但是生成的质量较差。

## controallable generation

懒得看了