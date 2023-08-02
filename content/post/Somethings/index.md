---
title: "Somethings"
date: 2023-08-02
license: CC BY-NC-ND
tags: ["others"]
categories : ["ML"]

---

## 关于为什么NLP中diffusion不火

谈及AIGC，CV or multimodal上最火的就是diffusion，但是在NLP中还是GPT为首的transformer autoregressive方式比较火，原因是什么。

*2023.8.2*

看了ICLR2023 “DIFFUSEQ: SEQUENCE TO SEQUENCE TEXT GENERATION WITH DIFFUSION MODELS” 引发的思考。

> 任务不同。multimodal的任务无非是生成图片，不论是unconditional还是conditional。而对于NLP，unconditonal生成seq并没有什么实际价值（胡言乱语有什么用呢）；而对于conditional，NLP的任务定义也与multimodal稍有不同：前者的condition是句子，而后者的condition通常为属性（比如说笑，哭，动漫化）。对后者来说，属性的种类有限，可以为每一个属性训练一个单独的classifier。而对前者，很难说每个句子都设计一个classifier。上面这篇文章解决的就是这个问题，把condition当作输入送入classifier-free diffusion，但是也只能做到和GPT-2 comparable。
>
> ICML2022“ On the Learning of Non-Autoregressive Transformers” 提到NAT方法在用max log likelihood时suffer from conditional total correlation。这篇文章我简单看了下，conditional total correlation大概就是说其实NAT的方法在生成当前token时没有考虑前一时刻的token（它同时生成整个序列的token的嘛），所以说并不会考虑token之间的相关性，而只会考虑此token在整个句子中的分布概率。所以说对于NLP的任务来说，AT天生好于NAT。
>
> 其实上述两个问题在ICLR这篇文章中都已经解决了（对后者他在diffusion中用的模型是transformer，即用AT的方式生成latent embedding）。可能也没有涉及为啥diffusion在NLP中不火的本质，但是这篇文章的效果确实跟transformer-based的方法比是有差距的。

这里有个问题，ICML这篇文章其实并没有指定NLP，他就是说用最大log相似作为目标的时候，NAT会有问题。但是DDPM这篇文章用的也是最大log相似（应该说生成任务用的都是最大log相似）并且不是AT模型，DDPM是否也存在这个问题？

