---
layout: post
title: 'Rademacher complexity and VC-dimension'
date: 2022-12-18 11:12:00-0400
description: xai
tags: XAI
categories: XAI-posts
---


## Definition


complexity measure of a hypothesis class. 
The empirical Rademacher complexity of a hypothesis class $\mathcal{H}$ on a dataset $\{ x_1, \cdots, x_n \}$ is defined as 

$$
\begin{equation}
\hat{\mathcal{R}}_n(\mathcal{H}) = \mathbb{E}_\sigma \Big[  \sup_{h\in \mathcal{H}} \frac{1}{n} \sum_{i=1}^n \sigma_i h(x_i) \Big]
\end{equation}
$$

where $\sigma_i \in \{ \pm 1 \}$ are i.i.d. uniform random variables. 

We have hypothesis class $\mathcal{H}$ which includes models to make predictions. 
For a given random $\sigma$, 



$$
{\displaystyle \operatorname {Rad} _{S}(F)=\operatorname {Rad} (F\circ S)}
$$


## Intuition

The hypothesis class could be understood as a space of functions. We check all the functions and there would be a suitable function for random labeling (samples from Rademacher distribution) if the functional space is large enough. For example a neural network [1].



## Rademacher distribution 

$\mathrm{Pr}(\sigma_i = +1) = \mathrm{Pr}(\sigma_i = -1) = 1/2$


## Reference

 [1] [Understanding deep learning requires rethinking generalization](https://arxiv.org/abs/1611.03530)
