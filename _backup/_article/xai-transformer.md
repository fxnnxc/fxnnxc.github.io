---
layout: post
title: 'Transformer Circuit [⚙️]'
date: 2023-06-14 00:00:00-0400
description: 'Test'
tags: Transformer 
category: XAI
subcategory : Circuits
---

# Features as basis 

* Decomposition : 

* Linearity :


## Privileged Basis


## Polysemantic



## Superposition 



## Soft Linear Units 

$$ 
\begin{equation}
\operatorname{SoLU}(x) = x * \operatorname{Softmax}(x)
\end{equation}
$$ 


# Singular Value Decomposition 


# Layer-Norm 

$$
\begin{equation} 
\operatorname{LayerNorm}[x] = \frac{x - \mathbb{E}[x]}{\sqrt{\operatorname{Var}[x] + \epsilon}} \cdot \gamma + \beta
\end{equation}
$$

[Torch LayerNorm](https://pytorch.org/docs/stable/generated/torch.nn.LayerNorm.html) supports element-wise affine transform.
n
```python
d=2
m = torch.nn.LayerNorm(d, elementwise_affine=False)
m = torch.nn.LayerNorm(d, elementwise_affine=True) # default
```


#### Prove that the norm is approximately $\sqrt{d}$

$$ 
\begin{align}
\Vert \operatorname{LN}[x] \Vert^2 &=  
\Vert x-\mathbb{E}[x] \Vert^2 / \Big( \operatorname{Var}[x] + \epsilon \Big) \\
&= \Vert x-\mathbb{E}[x] \Vert^2  /  \Big( 1/d \cdot \sum_i (x_i - E[x])^2   +\epsilon \Big) \\
&= d \cdot \Vert x-\mathbb{E}[x] \Vert^2  / \Big( \sum_i (x_i - E[x])^2   +\epsilon \Big) \\
& \approx d
\end{align}
$$

#### Examples 

$$ 
\begin{align*}
x &= [1,3]  \\
LN[x] &= [-1, 1] \cdot \gamma + \beta \\
~ \\
x &= [1,20,30]  \\
LN[x] &= [-1.3303,  0.2494,  1.0808] \cdot \gamma + \beta
\end{align*}
$$ 
