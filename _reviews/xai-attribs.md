---
layout: post
title: 'Attribution Methods in XAI [⚙️]'
date: 2023-04-09 00:00:00-0400
description: 'Attrib'
tags: XAI, Attrib
category: XAI
subcategory : Attrib
---


### Deconvolutional Networks

If the input to the ReLU during the forward pass is negative, then gradient signal is zero'd out. 

* Can not highlight negative signals

### Guided Backpropagation

zero's out the importance signal at a ReLU if either the input to the ReLU during the forward pass is negative or the importance signal during the backward pass is negative. 


* Can not highlight negative signals


---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Reference</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">DeepLIFT</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2018</tag>



### DeepLIFT 


DeepLIFT (Deep Learning Important FeaTures) assigns importance score to the inputs for a given output. 

*  importance in terms of differences from a 'reference' state 
* positive and negative contributions are modeled. 


#### Notations 

* $t$ : target output neuron of interest 
* $x_1, \cdots , x_n$ : some neurons in some intermediate layer or layers that are necessary and sufficient to compute $t$.
* $\Delta t = t - t^\mathrm{ref}$ : the difference-from-reference
* $C_{\Delta x_i \Delta t }$ : DeepLIFT contribution scores
* Summatation-to-delta 
$$
\sum_{i=1}^n C_{\Delta x_i \Delta t } = \Delta t
$$


#### Multipliers 

Given two neurons $x,t$, we wish to compute the contribution to, 

$$
m_{\Delta x \Delta t} = \frac{C_{\Delta x \Delta t }}{\Delta x}
$$

$m_{\Delta x \Delta t}$ is the contribution of $\Delta x$ to $\Delta t$ divided by $\Delta x$. 


#### Chain Rule 

Given input neurons $x_1, \cdots, x_n$, a hidden layer with neurons $y_1, \cdots, y_n$, and some target output $t$. 

$$
m_{\Delta x_i \Delta t} = \sum_j m_{\Delta x_i \Delta y_j} m_{\Delta y_i \Delta t}
$$

---