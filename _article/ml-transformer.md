---
layout: post
title: 'Transformer'
date: 2023-04-05 00:00:00-0400
description: ''
tags: DL, Transformer
category: DL
subcategory : Transformer
---


# Transformer 


### Zero Layer Transformer 

Elhage et al [1] showed the mathematical framework of a transformer to shed lights on the circuits of the model. Zero-layer-transformer is the most abstract form of the decoder. The model `inputs` tokens and `outputs` tokens. 
Given a word, the output is the prediction of the next word (bi-gram).


### Key-value Memory 


<Blockquote>
All the encoder blocks in layers contribute to the final vocabulary embedding by residual connection.
</Blockquote>

See [post](https://fxnnxc.github.io/reviews/nlp-knowledge-neurons/#:~:text=Knowledge-,Neurons,-Geva%20%5B1%5D%20argued) for details. 


### One Layer Transformer

* Attention : communication between tokens
* MLP : key-value neural memory for the output vocabulary
* Residual Connection : communication between channels. 

# References 

[1] Elhage, et al., "A Mathematical Framework for Transformer Circuits", Transformer Circuits Thread, 2021. [link](https://transformer-circuits.pub/2021/framework/index.html)