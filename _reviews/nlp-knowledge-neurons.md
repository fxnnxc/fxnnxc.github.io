---
layout: post
title: 'Knowledge Neurons in NLP'
date: 2023-04-04 00:00:00-0400
description: ''
tags: Neurons
category: NLP
subcategory : Neurons
---



# Knowledge Neurons 






---

<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Key-Value</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">Memory</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2019</tag>

Geva [1] argued the key-value memory structure in MLP layer of a transformer. 

#### Discovered Properties of Key-Value Memory 

* Lower layers tend to capture **shallow patterns** (existence of specific vocabulary), while upper layers learn more **semantic** ones (topics such as sports or TV shows). 
* The values complement the keys’ input patterns by inducing output distributions (each key is the weight for each value)
* the output of a feed-forward layer is **a composition of its memories**, which is subsequently **refined** throughout the model’s layers via residual connections to produce the final output distribution.


Motivated by `neural memory` [2] which consists of $m$ key-value pairs, which we call *memories*. 
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

#### Composition of Multiple Memories

In the transformer, the role of key is to capture patterns on the input sequence, and value represent distributions of tokens. 
Multiple memories weighted by keys are combined in a MLP.

$$
\large{\mathbf{y}^\ell = \sum_i \mathrm{ReLU}(\mathbf{x}^l \cdot \mathbf{k}_i^\ell) \cdot \mathbf{v}_i^\ell + \mathbf{b}^\ell}
$$

While a single feed-forward layer composes its memories in parallel, a `multi-layer model` uses the **residual connection** to sequentially compose predictions to produce the model’s final output. 


---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">IG</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">Selection</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2022</tag>


### Attribution of Neurons for Knowledge 

Dai et al. [3] proposed a way to compute the knowledge neurons for a factual relation, for example, 

<center>
<code><Kuwait, continent, Asia></code>
</center>

Define the probability of the correct answer predicted by a pretrained model 
with intervened signal $\hat{w}_i^{(l)}$ at the $i$-th neuron of a layer $l$ as 

$$
\mathbb{P}_x (\hat{w}_i^{(l)}) = p(y^*|x, w_i^{(l)} = \hat{w}_i^{(l)}) 
$$


Consider $\bar{w}_i^{(l)}$ is the original value calculated with the prompt $x$.
The attribution score of the neuron $w_i^{(l)}$ to the prediction is defined by 

$$
\mathrm{Attr}(w_i^{(l)}) = \bar{w}_i^{(l)} \int_{\alpha=0}^1 \frac{\partial \mathbb{P}_x(\alpha  w_i^{(l)}) }{\partial w_i^{(l)}} \mathrm{d}\alpha
$$

<Blockquote>
The authors use $m=20$ samples for computing the IG. 
</Blockquote>



### Refining Knowledge Neurons 

The authors removed the `false-positive` knowledge

For different prompts corresponding to the same fact, authors hypothesize that 
* they share the same set of `true-positive` knowledge neurons, since they express the same factual knowledge. 
* they do not share the `false-positive` knowledge neurons as long as the prompts are diverse enough.

The authors retain only neurons that are widely shared among these prompts. Given a relational fact, the process to identify its knowledge neurons is as follows. (Fact $\rightarrow$ Set of knowledge neurons)

1. Produce $n$ diverse prompts 
2. Calculate the knowledge attribution scores of neurons for each prompt 
3. Retain neurons with attribution scores greater than the attribution threshold $t$
4. Consider all the coarse sets together, retain the knowledge neurons shared by more than $p\%$ prompts. 

<Blockquote>
In practive, threshold is set to $0.2$ times the maximum attribution score and percentage $p=0.7$ initially, and increase and decrease it by $0.05$ until the average number of knowledge neurons lies in $[2,5]$.
</Blockquote>


---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Factual</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">Insertion</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2022</tag>


Meng et al. [4]


Given a pair of subject $s$ and object $o$, the proposed method finds corresponding $k_{\*}$ and $v_{\*}$ for subject and object respectively, and encode the pair in the weight matrix to edit the memory.  

### Finding Key $k_*$ for $s$
pass text $x$ containing the subject $s$ through model; then at layer $l^*$ and last subject token index $i$, read the activated values. 

Given subject $s$ and random prefix $x_j$, the input is $x=x_j + s$ and the key-representation is  

$$
k(x) = \sigma \Big( W_{k}^{(l^*)} \gamma (a_{[x],i}^{(l^*-1)} + h_{[x],i}^{(l^*-1)} ) \Big)
$$

The authors set $k_*$ as the average activation of the key activations

$$
k_* = \frac{1}{N} \sum_{j=1}^N k(x_j + s)
$$

<Blockquote>
In practice, random 50 token sequences of length 2 to 10 are used for $x_j$. 
</Blockquote>


### Finding Value $v_*$ for object $o$

Given new relation $(r, o^\*)$, and factual prompt $p$ of the form, for example, `"Space Needle is in downtown"` whose expected output of a GPT is `Seattle`, In this case, we want to change the output to $o^\*$ =`Paris`. To do so, we should find the corresponding value $v^\*$ representation that outputs $o^\*$. the value-representation $v_* = \mathrm{argmin}_z \mathcal{L}(z)$ is the one which minimizes the following objective.

$$
\mathcal{L}(z) = \frac{1}{N} \sum_{j=1}^N -\log \mathbb{P}_{G(m_i^{(l^*)}:=z)}[o^* | x_j + p] + D_\mathrm{KL} \Big( \mathbb{P}_{G(m_i^{(l^*)}:=z)}[x|p'] || \mathbb{P}_{G}[x|p']   \Big)
$$

There are two parts in minimization

<p>

<li>  $\mathbb{P}_{G(m_i^{(l^*)}:=z)}[o^* | x_j + p]$ : given random prefix and factual prompt $p$, find the $z$ representation which maximizes the likelihood of the object $o_*$ token.
</li> 
<li>  $D_\mathrm{KL} \Big( \mathbb{P}_{G(m_i^{(l^*)}:=z)}[x|p'] || \mathbb{P}_{G}[x|p']   \Big)$ : The right hand side of KL-divergence is the original output of the model, and the left hand side is the output of the interrupted output with value representation $z$. 
    <ul>
    $p'$ (of the form <code>"{subject} is a "</code>) prompt which encourages the understanding of the subject. 
    </ul>
</li>
</p>


### Inserting Memory  


The computed $(k_\*, v_\*)$ represents the full fact $(s,r,o^\*)$. Value MLP weight $W_\mathrm{value}$ is updated to $\hat{W}_\mathrm{value}$ with a rank-one update that inserts the new key-value association directly

$$
\hat{W}_\mathrm{value} k_* = v_* 
$$

In addition, the original key-value pair should be preserved. Let $K=[k_1 \| k_2 \|\cdots ]$ and $V=[v_1 \| v_2 \|\cdots ]$ be original key-value pairs for the Value matrix $W_\mathrm{value}$. Then, the minimization problem is  

$$
\mathrm{minimize}~ ||\hat{W}_\mathrm{value}K - V|| ~ \mathrm{such~ that}~ \hat{W}_\mathrm{value} k_* = v_*
$$

We can compute $\hat{W}_\mathrm{value}$ in closed form 

$$
\hat{W} = W + \Lambda (C^{-1}k_*)^\top
$$

where $C = KK^\top$ and $\Lambda = (v_\* - Wk_\*)/(C^{-1}k_*)^\top k_\* $




---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Massive</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">Insertion</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2023</tag>

Meng et al. [5]



# References 

[1] Geva, Mor, et al. "Transformer Feed-Forward Layers Are Key-Value Memories." Proceedings of the 2021 Conference on Empirical Methods in Natural Language Processing. 2021.


[2] Sukhbaatar, Sainbayar, Jason Weston, and Rob Fergus. "End-to-end memory networks." Advances in neural information processing systems 28 (2015).


[3] Dai, Damai, et al. "Knowledge Neurons in Pretrained Transformers." Proceedings of the 60th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers). 2022.


[4] Meng, Kevin, et al. "Locating and editing factual associations in gpt." Advances in Neural Information Processing Systems 35 (2022): 17359-17372.

[5] Meng, Kevin, et al. "Mass-editing memory in a transformer." arXiv preprint arXiv:2210.07229 (2022).