---
layout: post
title: 'Rademacher complexity and VC-dimension'
date: 2022-12-18 11:12:00-0400
description: xai
tags: XAI
category: ML
subcategory: Complexity
---


## Definition


complexity measure of a hypothesis class. 
The empirical Rademacher complexity of a hypothesis class $\mathcal{F}$ on a dataset $S = \{ x_1, \cdots, x_n \}$ is defined as 

$$
\begin{equation}
\hat{\mathcal{R}}_n(\mathcal{F}) = \mathbb{E}_\sigma \Big[  \sup_{f\in \mathcal{F}} \frac{1}{n} \sum_{i=1}^n \sigma_i f(x_i) \Big]
\end{equation}
$$

where $\sigma_i \in \{ \pm 1 \}$ are independent random variables drawn from the Rademacher distribution

$$
{\displaystyle \operatorname {Rad} _{S}(F)=\operatorname {Rad} (F\circ S)}
$$
where $F \circ S$ denotes function composition 

$$
F \circ S := \{ f(z_1), \cdots, f(z_m) | f \in F \}
$$



We have hypothesis class $\mathcal{F}$ which includes models to make predictions. 
If the function class is large enough, then we can find proper $f$ for each $\sigma$ that maximizes $\sigma f(x)$. 




## Intuition

The hypothesis class could be understood as a space of functions. We check all the functions and there would be a suitable function for random labeling (samples from Rademacher distribution) if the functional space is large enough. For example a neural network [1].



## Rademacher distribution 

$\mathrm{Pr}(\sigma_i = +1) = \mathrm{Pr}(\sigma_i = -1) = 1/2$


## Examples 


<div style='background-color:#F5FFFF; margin:2px;border: dashed; border-color: #4444;'>
<h3 style='text-align:center'> Example 1</h3>

$$
A = \{ (a,b) \} \in \mathbb{R}^2
$$

$$
\operatorname {Rad} (A) = \frac{1}{2} \cdot \Big(
    \frac{1}{2}\cdot (a+b) + \frac{1}{4}\cdot (a-b) + \frac{1}{4}\cdot (-a+b) + \frac{1}{4}\cdot (-a-b)
\Big) = 0
$$
</div>



<div style='background-color:#F5FFFF; margin:2px;border: dashed; border-color: #4444;'>

<h3 style='text-align:center'> Example 2</h3>

$$
A = \{ (1,1), (1,2) \} \in \mathbb{R}^2
$$

$$
\begin{align*}
\operatorname {Rad} (A) 
&= \frac{1}{2} \cdot \Big(
    \frac{1}{4} \cdot \max (1+1, 1+2) + \frac{1}{4} \cdot \max (1-1, 1-2)
    + \frac{1}{4} \cdot \max (-1+1, -1+2) + \frac{1}{4} \cdot \max (-1-1, -1-2)
\Big)\\ 
&= \frac{1}{8} (3+0+1-2) = \frac{1}{4}
\end{align*}
$$

</div>

## Reference

 [1] [Understanding deep learning requires rethinking generalization](https://arxiv.org/abs/1611.03530)
