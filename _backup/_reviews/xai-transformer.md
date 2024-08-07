---
layout: post
title: 'Transformer Circuits'
date: 2023-04-09 00:00:00-0400
description: 'Transformer'
tags: XAI, Transformer
category: XAI
subcategory : Transformer
---

### ⚠️ This page is under development. 



# Introduction 

This is a review of [A Mathematical Framework for Transformer Circuits](https://transformer-circuits.pub/2021/framework/index.html)[1] written by Elhage et al.  



This formulation includes Kronecker product as we multiply weight for individual tokens and tokens are mixed. See [Kronecker Product with Examples](/article/la-kronecher/) for the practice. 




# Zero-Transformer 

## Embedding and Unembedding Weights 

Token is a discrete representation. To change the one-hot form into a high dimensional representation, we have a embedding matrix $W_E \in \mathbb{R}^{n_{tokens} \times d_{model}}$ where $n_{tokens}$ is the number of tokens and $d_{model}$ is the size of internal representation. In most cases, $n_{tokens} \gg d_{model}\$. The output is again multiplied with unembedding matrix $W_{UN} \in \mathbb{R}^{d_{model} \times n_{tokens} }$.

The zero transformer is the most simplest form of a transformer which does not include the internal blocks and we just embed and unembed a token. 

$$
T = W_{UN} W_E
$$

Formally with Kronecker product to spread the weight matrix on tokens, we have  

$$
T = (\mathrm{Id}_{t} \otimes W_{UN}) \cdot (\mathrm{Id}_{t} \otimes  W_E)
$$


With the assumption that we train a model to predict the next token of a sentence, the zero transformer is same with a bi-gram model. 




# Residual Stream 


# Blocks 

The meaning of outputs of each block could be interpreted by weights for the vocabulary in $W_{UN}$





# MHA



The MHA operation is defined by 

$$
h(x) = (\mathrm{Id}_{t} \otimes W_O) 
\cdot (A \otimes \mathrm{Id}_{d}) 
\cdot (\mathrm{Id}_{t} \otimes W_v) \cdot x 
$$



where $t$ is the number of input tokens and $$\mathrm{Id}_{t}$$ is $$t\times t$$ identity matrix and $$\mathrm{Id}_{d}$$ is the $$d_{model} \times d_{model}$$ identity matrix. The operation $$(\mathrm{Id}_{t} \otimes W_O)$$ could be understood as spreading $$W_O$$ weight matrix to each token and $$(A \otimes \mathrm{Id}_{d})$$ is spreading $$d_{model}$$ dimensions for each attention scores so that the attention score, which is a scalar, can be expanded as a $$d_{model}$$ dimensional vector.  See <a href="#example"> #The Meaning of Kronecker Product in Multi-Heads </a> for additional information. 




The MHA operation $h(x)$ can be simplified as  $\Big( A  \otimes (W_O \cdot W_v )\Big) $


<div style='background-color:#F5FFFF; margin:2px;border: dashed; border-color: #4444; text-align:center;'>

<h3>
<i>Proof) </i>
</h3>

Because of the mixed product of Kronecker product, the equation following holds
$$
{(A \otimes B ) \cdot (C \otimes D) = (A \cdot B) \otimes (C \cdot D)}
$$


Then, $h(x)$ can be written as follows.

$$
\begin{align*}
h(x) 
&= (\mathrm{Id}_{t} \otimes W_O)  \cdot (A \otimes \mathrm{Id}_{d} ) \cdot (\mathrm{Id}_{t} \otimes W_v) \cdot x \\
&= \Big( (\mathrm{Id}_{t} \otimes W_O)  \cdot (A \otimes \mathrm{Id}_{d} ) \Big) \cdot (\mathrm{Id}_{t} \otimes W_v) \cdot x \\
&= \Big((\mathrm{Id}_{t} \cdot A) \otimes (W_O \cdot \mathrm{Id}_{d} )\Big)  \cdot (\mathrm{Id}_{t} \otimes W_v) \cdot x \\
&= \Big((\mathrm{Id}_{t} \cdot A \cdot \mathrm{Id}_{t}) \otimes (W_O \cdot \mathrm{Id}_{d} \cdot W_v )\Big)  \cdot x \\
&= \Big( A  \otimes (W_O \cdot W_v )\Big)  \cdot x
\end{align*} 
$$

</div>



<h2 id="example"> The Meaning of Kronecker Product in Multi-Heads
</h2>

<div  style='background-color:#FFFFF5; margin:2px;border: dashed; border-color: #4444; text-align:center;'>


<h3>
<i>Example) </i>
</h3>

Let $ t=3,d=2 $ be the number of tokens and dimension each, and assume that we have 
$ x = \left( \begin{array}{cc|cc|cc} \textcolor{blue}{1} & \textcolor{blue}{2} &  \textcolor{green}{3} & \textcolor{green}{4}  & \textcolor{red}{5} & \textcolor{red}{6} \end{array}\right)^\top $ input where each color indicate separate token and 

$$
\mathrm{Id}_t = \begin{pmatrix}
  1 & 0 & 0 \\
  0 & 1 & 0 \\
  0 & 0 & 1 
\end{pmatrix}, 
\mathrm{Id}_d = \begin{pmatrix}
  1 & 0 \\
  0 & 1  
\end{pmatrix}, 
W_O = \begin{pmatrix}
  -1 & -2 \\
  -3 & -4  
\end{pmatrix}, 
A = \begin{pmatrix}
  0.3 & 0.2 & 0.5 \\
  0.1 & 0.6 & 0.3 \\
  0.2 & 0.1 & 0.7
\end{pmatrix}
$$

Then, 

$$
\begin{align*}
\mathrm{Id}_{t} \otimes W_O 
&=
\begin{pmatrix}
  1 & 0 & 0 \\
  0 & 1 & 0 \\
  0 & 0 & 1 
\end{pmatrix} \otimes \begin{pmatrix}
  -1 & -2 \\
  -3 & -4  
\end{pmatrix} \\
&= \begin{pmatrix}
  -1 & -2 & 0 & 0  & 0 & 0 \\
  -3 & -4 & 0 & 0  & 0 & 0 \\
  0 & 0 & -1 & -2  & 0 & 0 \\
  0 & 0 & -3 & -4  & 0 & 0 \\
  0 & 0 & 0 & 0  & -1 & -2 \\
  0 & 0 & 0 & 0  & -3 & -4 
\end{pmatrix} 
\end{align*}
$$

$$
\begin{align*}
A \otimes \mathrm{Id}_{d}  = 
&= \begin{pmatrix}
  0.3 & 0.2 & 0.5 \\
  0.1 & 0.6 & 0.3 \\
  0.2 & 0.1 & 0.7
\end{pmatrix} \otimes \begin{pmatrix}
  1 & 0 \\
  0 & 1  
\end{pmatrix} \\
&=  \begin{pmatrix}
  0.3 & 0   & 0.2 & 0   & 0.5 & 0  \\
  0   & 0.3 & 0   & 0.2 & 0   & 0.5  \\
  0.1 & 0   & 0.6 & 0   & 0.3 & 0  \\
  0   & 0.1 & 0   & 0.6 & 0   & 0.3  \\
  0.2 & 0   & 0.1 & 0   & 0.7 & 0  \\
  0   & 0.2 & 0   & 0.1 & 0   & 0.7 
\end{pmatrix}  
\end{align*}
$$
</div>

<div style='background-color:#FFFFF5; margin:2px;border: dashed; border-color: #4444; text-align:center;'>

<h3 style="text-align:left"> 1) Multiply representations of tokens to $\mathrm{Id}_{t} \otimes W_O $ and  $A \otimes \mathrm{Id}_{d}$ </h3>

Now consider  the meanings of outputs for each Kronecker product 
$$
\Big(\mathrm{Id}_{t} \otimes W_O\Big) \cdot x 
= \begin{pmatrix}
  \textcolor{blue}{-1} & \textcolor{blue}{-2} & 0 & 0  & 0 & 0 \\
  \textcolor{blue}{-3} & \textcolor{blue}{-4} & 0 & 0  & 0 & 0 \\
  0 & 0 & \textcolor{green}{-1} & \textcolor{green}{-2}  & 0 & 0 \\
  0 & 0 & \textcolor{green}{-3} & \textcolor{green}{-4}  & 0 & 0 \\
  0 & 0 & 0 & 0  & \textcolor{red}{-1} & \textcolor{red}{-2} \\
  0 & 0 & 0 & 0  & \textcolor{red}{-3} & \textcolor{red}{-4} 
\end{pmatrix} \cdot \begin{pmatrix}
\textcolor{blue}{1} \\ \textcolor{blue}{2} \\ \hline \textcolor{green}{3} \\ \textcolor{green}{4} \\ \hline \textcolor{red}{5} \\ \textcolor{red}{6}
\end{pmatrix}
$$
and 
$$
\Big(A \otimes \mathrm{Id}_{d} \Big) \cdot x = \begin{pmatrix}
  \textcolor{blue}{0.3} & 0   & \textcolor{green}{0.2} & 0   & \textcolor{red}{0.5} & 0  \\
  0   & \textcolor{blue}{0.3} & 0   & \textcolor{green}{0.2} & 0   & \textcolor{red}{0.5}  \\
  0.1 & 0   & 0.6 & 0   & 0.3 & 0  \\
  0   & 0.1 & 0   & 0.6 & 0   & 0.3  \\
  0.2 & 0   & 0.1 & 0   & 0.7 & 0  \\
  0   & 0.2 & 0   & 0.1 & 0   & 0.7 
\end{pmatrix} \cdot \begin{pmatrix}
\textcolor{blue}{1} \\ \textcolor{blue}{2}  \\  \hline \textcolor{green}{3} \\ \textcolor{green}{4} \\ \hline  \textcolor{red}{5} \\ \textcolor{red}{6}
\end{pmatrix}
$$
</div>


<div style='background-color:#FFFFF5; margin:2px;border: dashed; border-color: #4444; text-align:center;'>

<h3 style="text-align:left"> 2) Mixing Transformed information with $A \otimes W_O W_V$ </h3>


$$
\begin{align*}
\Big(A \otimes W_O W_V\Big)  \cdot x
&= \begin{pmatrix}
  0.3 & 0.2 & 0.5 \\
  0.1 & 0.6 & 0.3 \\
  0.2 & 0.1 & 0.7
\end{pmatrix} \otimes 
\begin{pmatrix}
  a & b \\
  c & d
\end{pmatrix} \cdot x \\
&=
\left(
\begin{array}{cc|cc|cc}
  \textcolor{blue}{0.3} a & \textcolor{blue}{0.3} b & \textcolor{green}{0.2} a & \textcolor{green}{0.2} b & \textcolor{red}{0.5} a & \textcolor{red}{0.5} b \\
  \textcolor{blue}{0.3} c & \textcolor{blue}{0.3} d & \textcolor{green}{0.2} c & \textcolor{green}{0.2} d & \textcolor{red}{0.5} c & \textcolor{red}{0.5} d \\
  \textcolor{blue}{0.1} a & \textcolor{blue}{0.1} b & \textcolor{green}{0.6} a & \textcolor{green}{0.6} b & \textcolor{red}{0.3} a & \textcolor{red}{0.3} b \\
  \textcolor{blue}{0.1} c & \textcolor{blue}{0.1} d & \textcolor{green}{0.6} c & \textcolor{green}{0.6} d & \textcolor{red}{0.3} c & \textcolor{red}{0.3} d \\
  \textcolor{blue}{0.2} a & \textcolor{blue}{0.2} b & \textcolor{green}{0.1} a & \textcolor{green}{0.1} b & \textcolor{red}{0.7} a & \textcolor{red}{0.7} b \\
  \textcolor{blue}{0.2} c & \textcolor{blue}{0.2} d & \textcolor{green}{0.1} c & \textcolor{green}{0.1} d & \textcolor{red}{0.7} c & \textcolor{red}{0.7} d 
\end{array} \right) \cdot 
 \begin{pmatrix}
\textcolor{blue}{1} \\ \textcolor{blue}{2}  \\  \hline \textcolor{green}{3} \\ \textcolor{green}{4} \\ \hline  \textcolor{red}{5} \\ \textcolor{red}{6}
\end{pmatrix}
\end{align*}
$$

</div>

# One-Layer Transformer (attention only)

$$
T^1_\mathrm{attn-only} = (\mathrm{Id} \otimes W_U) \cdot 
\Big( 
\mathrm{Id} + \sum_{h\in H_1} A^h \otimes W^h_{OV}
\Big)
\cdot 
(\mathrm{Id}\otimes W_E)
\cdot
$$

where Multihead Attention is 

$$
A^h = \mathrm{softmax}^* \Big( t^\top W_E^\top W_{QK}^h W_E \cdot t  \Big)
$$ 

with autoregressive masking softmax operation, $$\mathrm{softmax}^*$$. By applying distributive property of Kronecker Product, we have

$$
T^1_\mathrm{attn-only} = \mathrm{Id} \otimes W_U W_E + \sum_{h\in H} A^h \otimes (W_U W_{OV}^h W_E )
$$

There are two operations: Residual connection of Zero Transformer and Block operation.  


# MLP





# Transformer Equation


Let $x^{(l)}$ be $l$-th output of transformer layers, $(l+1)$-th output is written as 

$$
\begin{align*}
a^{(l+1)} &= \operatorname{MHA}(\operatorname{LN}(x^{(l)})) \\ 
m^{(l+1)} &= \operatorname{MLP}(\operatorname{LN}(a^{(l+1)})) \\
x^{(l+1)}   &= x^{(l)} + m^{(l+1)} 
\end{align*}
$$

where $\operatorname{MLP}, \operatorname{MHA}$ and  $\operatorname{LN}$ are Multi-Layer Perceptron (MLP), Multi-Head Attention (MHA) and Layer-Norm (LN) each. 





# One Layer Transformer Recap 

Attention OV circuits for heads have positive eigenvalues. 

OV circuits determines how the values are transformed after the token representations are mixed. 




# Two Layer Transformer 

$$
T^2_\mathrm{attn-only} = (\mathrm{Id} \otimes W_U) \cdot 
\Big( 
\mathrm{Id} + \sum_{h\in H_2} A^h \otimes W^h_{OV}
\Big)
\cdot
\Big( 
\mathrm{Id} + \sum_{h\in H_1} A^h \otimes W^h_{OV}
\Big)
\cdot 
(\mathrm{Id}\otimes W_E)
\cdot
$$

$$
T^2_\mathrm{attn-only} = (\mathrm{Id} \otimes W_U W_E)
+ 
 \sum_{h\in H_1} A^h \otimes (W_U W^h_{OV} W_E)
 +
 \sum_{h\in H_2} A^h \otimes (W_U W^h_{OV} W_E)
+
 \sum_{h_2 \in H_2 } \sum_{h_1 \in H_1} (A^{h_2} A^{h_1}) \otimes (W_U W_{OV}^{h_2} W_{OV}^{h_1} W_E)
$$


Q-composition : $\sum_{h_q \in H_1} {(W_OV^{h_q})}^\top W_{QK}^{h_2}$
K-composition : $\sum_{h_k \in H_1}  W_{QK}^{h_2} {(W_OV^{h_q})}^\top $
V-composition : $ W_{OV}^{h_2} (W_OV^{h_1})$


* One attention communication (-5.2 nats)
* Direct Path (-1.3 nats)
* Virtual Communication is less important (-0.3 nats)


* Perturbing layer 1 has little effect. 
* Perturbing layer 2 has more effect. 


Find and copy is possible


[a][b] ... [a] -> [b]

Step 1) [a] find [a] in the previous steps 
Step 2) [b] information is passed to the output. 

If the pattern is not found, the finding goes to the first part. 





# Refernces 

[1] https://transformer-circuits.pub/2021/framework/index.html