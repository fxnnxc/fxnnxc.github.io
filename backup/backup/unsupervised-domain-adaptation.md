---
layout: post
title: 'Unsupervised Domain Adaptation'
date: 2022-12-19 11:12:00-0400
description: unsupervised
tags: RepresentationLearning
categories: RepresentationLearning
---


We have images and corresponding labels in source domain and in target domin. 
Transfering models is called Unsupervised Domain Adaptation (UDA)

Two methods 

* Domain Alignment (DA) : deep features, ignores the class features
* Class Alignment (CA)  : consider the class features


we need more search space for CA than DA. 


## Formulation 

Segmentation tasks 
input space $\mathcal{X} \in \mathbb{R}^{H\times W \times 3}$ and the label space 
$\mathcal{Y} \in \{0,1\}^{H\times W \times K}$, representing the ground-truth $K$-class segmentation images. 

A *hypothesis* is a function $h : \mathcal{X} \rightarrow \mathcal{Y}$. We denote the space of $h$ as $\mathcal{H}$. With the loss function $l(\cdot, \cdot)$, the expected error of $h$ on $\mathcal{D}_S$ is defined as 

$$
\epsilon_S (h) := \mathbb{E}_{(x,y)\sim \mathcal{D}_S} l(h(x) , y)
$$

Similary, the error of $h$ on $\mathcal{D}_T$ is 

$$
\epsilon_T (h) := \mathbb{E}_{(x,y)\sim \mathcal{D}_T} l(h(x) , y)
$$


The authors in [1] propose a theorem 

<blockquote>
For a hypothesis $h$, 
$$
\epsilon_T(h) \le \epsilon_S (h) + d_1(\mathcal{D}_S, \mathcal{D}_T) + \lambda 
$$
where $d_1$ is the $L^1$ divergence for two distributions, and the constant term $\lambda$ does not depend on any $h$. 

</blockquote>

## References 


[1] CALI : Coarse-to-Fine ALIgnments Based Unsupervised Domain Adaptation of Traversability Prediction for Deployable Autonomous Navigation, Chen et al, RSS 2022. 










