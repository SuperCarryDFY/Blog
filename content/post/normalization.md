---
title: "Sorts of Normalization"
date: 2023-08-02
license: CC BY-NC-ND
tags: ["others"]
categories : ["ML"]B
---

## BatchNorm

BatchNorm need to 

## LayerNorm

LayerNorm is mostly used in NLP. Because the length of the sentences is not always same, so batchnorm is not suitable. LayerNorm normalizes the input along the word-dimention.

>  Noted that the input is [bs, length,embeddings]. LayerNorm normalizes the embeddings for each words.

```python
torch.nn.LayerNorm(
        normalized_shape: Union[int, List[int], torch.Size],
        eps: float = 1e-05,
        elementwise_affine: bool = True)
```

to be continue...
