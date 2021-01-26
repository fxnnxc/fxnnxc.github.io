---
title: 'Inference of the Transformer is tricky!'
date: 2021-01-21
permalink: /posts/2021/01/transformer-inference/
tags:
  - training
  - transformer
  - bart
  - infernece
---

## Inference of the transformer

Inference stage of the transformer architecture is different with RNN case. 

When we encode a source sentence, we get a hidden state of the source. 

When we decode that hidden state, we should generate tokens one by one. Therefore we run encoder only 1 time but decoder several times.

Because of this property, we don't use the forward method in the transformer, but instead we use forward method in the encoder and the decoder each. 

In the fairseq code, this happens in ```Sequence Generator``` class. If you want to decode tokens with additional layers, then change the ```def generate``` code to use additional layers.  