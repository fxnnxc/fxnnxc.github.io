---
layout: distill
title: 'Neural Network Editing Study '
date: 2023-09-04
description: Study of network editing. 
img: 'https://drive.google.com/uc?export=view&id=1GfMXX5JbJJ6q4TswUhcNjL-zgyEGKJu7'
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: false

---

> This post is a personal study of editing neural networks

## Introduction 

This post is about my practice on the mathematical formulation of editing. This post covers maths in [📃 *Rewriting a deep generative model*]<d-cite key="bau2020rewriting"/> which are further used in 
* [📃  *Editing a classifier by rewriting its prediction rules*]<d-cite key="santurkar2021editing"/>
* [📃  *Locating and editing factual associations in GPT*, Factual GPT]<d-cite key="meng2022locating"/>
* [📃 *Unified Concept Editing in Diffusion Models*]<d-cite key="gandikota2023unified"/>



## Keywords Summary


* 🔖 *model rewriting* : task which aims to add, remove, and alter the semantic and physical rules of a pretrained deep network. 
* 🔖 *linear associative memory* : relationship between two values are defined as linear transform. input as key and the output as value. 







## Example 


We have initial weight $w_0$ which is further rewritten with another weights $\Delta$ by 

1. $w_0 + \Delta_1$ : remove text in image 
2. $w_0 + \Delta_1 + \Delta_2$ : add people
3. $w_0 + \Delta_1 + \Delta_2 + \Delta_3$ : replace tower by tower


<figure>
<img src="https://drive.google.com/uc?export=view&id=1m8m0GHn7NAgDJNjIieVSuyomec1M18-d">
<figcaption>
Figure from the paper "Rewriting a deep generative model"
</figcaption>
</figure>



## Formulation of Editing 

A pre-trained generator $G(z;\theta_0)$ with weights $\theta_0$ and input latent $z$, generate images $x_i = G(z_i;\theta_0)$ is produced by a latent code $z_i$. As we want to edit $x_i$ to desired image $$x_i^*$$, we would update weights $\theta_1$ that change a computational rule to match $x_i^* \approx G(z_i; \theta_1)$ with regularization 

<div style="width:100%; overflow-x:clip; overflow-y:clip;">

$$
\begin{gather}
\theta_1 = \arg \min_\theta \mathcal{L}_\mathrm{smooth}(\theta) + \lambda \mathcal{L}_\mathrm{constraint}(\theta)\\ 
\mathcal{L}_\mathrm{smooth}(\theta) =  \mathbb{E}_z [\ell(G(z;\theta_0), G(z; \theta))] \\ 
\mathcal{L}_\mathrm{constraint}(\theta) =  \sum_i \ell(x_i^*, G(z_i; \theta))
\end{gather}
$$

</div>

Equation 2 ensures little difference between two operations $(G(z;\theta_0)$ and  $G(z; \theta)$ and Equation 3 ensures the generated image with $z_i$ is the desired image $$x_i^*$$.

However, this technique requires the modification of all weight $\theta$ and the novel idea proposed by Bau is the modification of only one layer<d-footnote> In the paper, the authors said "we reduce the degrees of freedom by modifying weights at only one layer." </d-footnote>.


Let $v = f_l(k;W_0)$ denote the computation of layer $L$ with weights $W_0$. For each exemplar latent $z_i$, theses layers produce features $$k_i^*$$ and $$v_i^* = f_l(k_i^*;W_0)$$ Here, $$v_i^*$$ is the latent for the desired image $$x_i^*$$ which could be obtained via such as GAN inversion. The objective Equation 1 becomes

<div style="width:100%; overflow-x:clip; overflow-y:clip;">

$$
\begin{gather}
W_1 = \arg \min_W \mathcal{L}_\mathrm{smooth}(W) + \lambda \mathcal{L}_\mathrm{constraint}(W)\\ 
\mathcal{L}_\mathrm{smooth}(W) =  \mathbb{E}_k [\Vert  f_l(k;W_0), f_l(k; W)\Vert^2] \\ 
\mathcal{L}_\mathrm{constraint}(W) =  \sum_i \Vert (v_i^*, f_l(k_i; W) \Vert^2 
\end{gather}
$$

</div>

The editing of generative model is done by learning weights to match $$k_i^*$$ (given input from $$z_i$$)  and  $$v_i^*$$ (desired feature for $$x_i^*$$).

> In conclusion, modifying weights in a layer to map $k_i^* \rightarrow v_i^*$.



## Weight Matrix Update


Matrix $W$ can be viewed as an association memory. The term association means the memory determines the association of two values, *inputs* and *outputs*. 
A set of key-value pairs $\{ (k_i, v_i) \}$ has the following association with memory $W$. 

$$
v_i \approx W k_i
$$

If the keys are (1) mutually orthogonal and (2) error-free, then the memory can be recovered in *error-free* manner.


<div style="width:100%; overflow-x:clip; overflow-y:clip;">

$$
\begin{equation}
W_\mathrm{orth} \triangleq \sum_i v_i k_i^\top
\end{equation}
$$

</div>


With this matrix, we have the following property 

<div style="width:100%; overflow-x:clip; overflow-y:clip;">

$$
\begin{aligned}
W_\mathrm{orth} k_j 
&= (\sum_i v_i k_i^\top) k_j  \\ 
&= v_j k_j^\top k_j + \sum_{i\ne j} v_i k_i^\top k_j  \\
&= v_j \cdot 1 +  \sum_{i\ne j} v_i 0  \\
&= v_j
\end{aligned}
$$

</div>

The below example shows the editing of convolutional layer which produces $$v = W_\mathrm{reshaped} k_\mathrm{reshaped}$$. 

* Weight $W$  : convolutional weight  (`256 x 512 x 3 x 3` dim / reshaped : `2304 x 512`)
* Key $k$ : a single feature vector matches **the similar location semantic** (`512` dim )
* Value $v$ : **rendered shape semantic** (`256 x 3 x 3` dim / reshaped: `2304`)

<figure>
<img src="https://drive.google.com/uc?export=view&id=1GfMXX5JbJJ6q4TswUhcNjL-zgyEGKJu7" >
<figcaption>
Figure from the paper "Rewriting a deep generative model"
</figcaption>
</figure>


### Nonorthogonal keys


Orthogonal keys have at most $N$ number of keys where $N$ is length of columns. In other words, only $N$ number of semantic keys are possible. However, the semantic space could have more number of semantic keys and nonorthogonal encode more key-value pairs with 🌟**superpositions**.


Let $N'$ be the number of nonorthogonal keys $\{k_i\}. We choose $W_0$ to minimize error:


<div style="width:100%; overflow-x:clip; overflow-y:clip;">
$$
\begin{equation}
W_0 \triangleq  \arg \min_W \sum_{i} \Vert v_i - Wk_i \Vert^2
\end{equation}
$$
</div> 

For the simplification of notations, we can stack the key and values columns to form matrices 


<div style="width:100%; overflow-x:clip; overflow-y:clip;">

$$
\begin{gather}
K \triangleq  [k_1 | k_2 | \cdots | k_i | \cdots | k_{N'}  ] \\ 
V \triangleq  [v_1 | v_2 | \cdots | v_i | \cdots | v_{N'}  ]
\end{gather}
$$

</div>

<div style="width:100%; overflow-x:clip; overflow-y:clip;">

$$
\begin{align}
W_\mathrm{0} &=  \arg \min_W \Vert V - WK \Vert^2 \\
\Rightarrow&  W_0 KK^\top = VK^\top \\ 
\Rightarrow&  W_0 = VK^\top (KK^\top)^{-1} \\ 
\Rightarrow&  W_0 = VK^+ \\ 
\end{align}
$$

</div>

where $K^+$ is the pseudo-inverse of $K$ [[see wiki](https://en.wikipedia.org/wiki/Moore%E2%80%93Penrose_inverse)]. 


### Insert New Value 


<div style="width:100%; overflow-x:clip; overflow-y:clip;">
$$
\begin{align}
W_1   &= \min_W \sum_{i} \Vert v_i - Wk_i \Vert^2 \\ 
& \mathrm{subject~to~} v_* = W_1 k_*
\end{align}
$$
</div> 

It is known that Equation 15 with constraint Equation 16 can be solved exactly as $$W_1KK^\top  = VK^\top + \Lambda k_*^\top$$ where $$\Lambda \in \mathbb{R}^{m}$$ is determined to satisfy the constraint in Equation 16. 


<div style="width:100%; overflow-x:clip; overflow-y:clip;">
$$
\begin{align}
W_1KK^\top  
&= VK^\top + \Lambda k_*^\top \\
W_1 &= W_0 + \Lambda((KK^\top)^{-1}k_*)^\top \\
W_1 &= W_0 + \Lambda(C^{-1}k_*)^\top \\
W_1 &= W_0 + \Lambda d^\top \\
\end{align}
$$
</div>

As $\Lambda$ is a vector and $d$ is also a vector, the modification updates $W_0$ to $W_1$ with rank one matrix. This is why editing with this method is called **Rank One Model Editing (ROME)**.



### Nonlinear Neural Layer 

The nonlinear means that the value is not just the output of a matrix but the non-linearly modified value with activations such as ReLU and Tanh. The authors also generated the insertion as follows.

The key direction is the multiplication of the inverse of covariance matrix $C^{-1}$ and the desired key $k_*$. 

<div style="width:100%; overflow-x:clip; overflow-y:clip;">
$$
\begin{equation}
d \triangleq C^{-1} k_*
\end{equation}
$$
</div>

The component $\Lambda$ for the row space is found by the optimization process. 

<div style="width:100%; overflow-x:clip; overflow-y:clip;">
$$
\begin{align}
\Lambda_1  = \arg \min_{\Lambda \in \mathbb{R}^M} \Vert v_* - f(k_*;W_0 + \Lambda d^\top) \Vert \\
\Rightarrow W_1 = W_0 + \Lambda_1 d^\top 
\end{align}
$$
</div>

The update weight is same with Equation 20. 

<div style="width:100%; overflow-x:clip; overflow-y:clip;">
$$
\begin{align}
W_1 &= W_0 + \Lambda d^\top 
\end{align}
$$
</div>

There are situations where more number of values should be updated at once. 
More generalized number of modification is the matrix version of finding components.

<div style="width:100%; overflow-x:clip; overflow-y:clip;">
$$
\begin{align}
\Lambda_S  &= \arg \min_{\Lambda \in \mathbb{R}^{M\times S}} \Vert V_* - f(K_*;W_0 + \Lambda D_S^\top) \Vert \\ 
& \Rightarrow W_S = W_0 + \Lambda_S D_S^\top 
\end{align}
$$
</div>

where $$D_S = [d_1 \vert d_2 \vert \cdots \vert d_S]$$.

---

## Editing Classifier 

I summarized briefly the core mechanism of  [📃  *Editing a classifier by rewriting its prediction rules*]<d-cite key="santurkar2021editing"/>



### Method 

Given an original image $x$, the original output $y$ is produced by the classifier and the internal representation value $v$ which may have semantic meaning. This value $v$ is the representation that already mapped to the correct label $y$. 

Now assume that we have modified input $x'$ and we want to map it to the value $v$ to produce label $y$. To do this, the *linear association memory* is applied to map the new key to desired the value 


<div style="width:100%; overflow-x:clip; overflow-y:clip;">
$$
\begin{align}
k_* \rightarrow v_*
\end{align}
$$
</div>

For example, the original car image has semantic wheel which is mapped to the label *Car* or *Bicycle* and leaves the value $v^\*$ from the *wheel* concept. When we want edit the wheel concept to *wooden wheel*, but preserves the prediction as *Car* and *Bicycle*, the new key *wooden wheel* needs to be mapped to the value $v^*$.

<figure>
<img src="https://drive.google.com/uc?export=view&id=1X-qMZZv5IWQZuCu0U5SWms2AL2DTa5HU" style="width:120%"  >
<figcaption>
Figure from the paper "Editing a classifier by rewriting its prediction rules"
</figcaption>
</figure>

### Why We Edit Association (Key $\rightarrow$ Value)? 

Because there is spurious correlation between backgrounds in classification, the prediction could be biased. For example, **snow** concept should be mapped to the value of **road** to be invariant to the backgrounds.  

<figure>
<img src="https://drive.google.com/uc?export=view&id=1GqxOKT_w5DatOa-Z77RJqrGwBOlG9-tW" style="width:120%" >
<figcaption>
Figure from the paper "Editing a classifier by rewriting its prediction rules"
</figcaption>
</figure>
