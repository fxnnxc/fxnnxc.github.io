---
layout: post
title: 'Knowledge Neurons in NLP'
date: 2022-05-08 00:00:00-0400
description: ''
tags: Neurons
category: NLP
subcategory : Neurons
---



# Knowledge Neurons 


Geva [1] argued the key-value memory structure in MLP layer of a transformer. 

#### Discovered Properties of Key-Value Memory 

* Lower layers tend to capture shallow patterns, while upper layers learn more semantic ones. 
* The values complement the keys’ input patterns by inducing output distributions
* the output of a feed-forward layer is a composition of its memories, which is subsequently refined throughout the model’s layers via residual connections to produce the final output distribution.


Motivated by neural memory [2] which consists of $m$ key-value pairs, which we call *memories*. 
* `key`: $d$-dimensional vector $\mathbf{k}_i \in \mathbb{R}^d$, and together form the parameter matrix $K \in \mathbb{R}^{m\times d}$. The authors compute memory coefficient by $\text{ReLU}(\mathbf{x}_j^l \cdot \mathbf{k}_i^\ell)$ for a key $k_i^l$ that corresponds to the $i$-th hidden dimension of the $\ell$-th feed-forward layer. To validate the claim, they showed top-25 inputs which triggers the memory. 
* `value` : paramter $V\in \mathbb{R}^{m\times d}$. The value is directly related to the probability distribution over the vocabulary by multiplying it by  the output embedding matrix $E$:

$$
\large{\mathbf{p}_i^\ell = \mathrm{softmax}(\mathbf{v}_i^\ell \cdot E)}
$$

The conditional distribution of keys are 
$$
p(\mathbf{k}_i|\mathbf{x}) \propto \exp(\mathbf{x}\cdot \mathbf{k}_i)
$$
and memory is a linear combination of values 

$$
\large{\begin{align}
\mathrm{MN}(\mathbf{x}) &= \sum_{i=1}^m p(\mathbf{k}_i|\mathbf{x}) \mathbf{v}_i  \\
&= \mathrm{softmax}(\mathbf{x} \cdot K^\top)\cdot V
\end{align}}
$$

* *hidden layer* : $\mathbf{m} = f(\mathbf{x} \cdot K^\top)$
* *memory coefficient* of the ith memory cell : $\mathbf{m}_i$

In the transformer, the role of key is to capture patterns on the input sequence, and value represent distributions of tokens. 
Multiple values are contribute by composition of memories.

$$
\large{\mathbf{y}^\ell = \sum_i \mathrm{ReLU}(\mathbf{x}^l \cdot \mathbf{k}_i^\ell) \cdot \mathbf{v}_i^\ell + \mathbf{b}^\ell}
$$

While a single feed-forward layer composes its memories in parallel, a multi-layer model uses the residual connection r to sequentially compose predictions to produce the model’s final output. 



# References 

[1] Geva, Mor, et al. "Transformer Feed-Forward Layers Are Key-Value Memories." Proceedings of the 2021 Conference on Empirical Methods in Natural Language Processing. 2021.


[2] Sukhbaatar, Sainbayar, Jason Weston, and Rob Fergus. "End-to-end memory networks." Advances in neural information processing systems 28 (2015).