---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-07-31
featured: true
title: 'Understand the interpretation of Othello '
description: ''
---

> This post is a personal analysis of a paper: Emergent World Representations: Exploring a Sequence Model Trained on a Synthetic Task. 

Recently Li et al. applied mechanistic interpretation of GPT to Othello game <d-cite key="li2022emergent">.



* [Paper:Emergent World Representations](https://arxiv.org/abs/2210.13382)
* [github](https://github.com/likenneth/othello_world)
* [OthelloScope](https://kran.ai/othelloscope/L2/N260/index.html), [[post](https://apartresearch.itch.io/othelloscope)]


## Probing Neurons 

For a given neuron in a GPT model, the goal of probing is the verification of alignment between the concept and the activation of a neuron. Consider a set of neurons and the activations of several samples. When we assign labels on the activations, we can train a model to predict the relationship between activation patterns and the concepts. 


Consider a set of activations over neurons for $N$ number of samples. When we stack them, we have the following form 


$$
\begin{bmatrix}
y_1 \\ y_2 \\ y_3 \\ \vdots \\ y_N
\end{bmatrix}
=
\begin{bmatrix}
a_{11} & a_{12} & a_{13} & a_{14} & \cdots & a_{1d} \\
a_{21} & a_{22} & a_{23} & a_{24} & \cdots & a_{2d} \\
a_{31} & a_{32} & a_{33} & a_{34} & \cdots & a_{3d} \\
&& \vdots \\
a_{N1} & a_{N2} & a_{N3} & a_{N4} & \cdots & a_{Nd} \\
\end{bmatrix}
$$


### Linear 

The linear probing assumes that there are linear relationship between the activation of a neuron and the label so that we can train a linear model with parameter $$\theta = \{ W \in \mathbb{R}^{d\times1} \}$$. The function is the form of $y = Wa$. 

$$
\begin{bmatrix}
y_1 \\ y_2 \\ y_3 \\ \vdots \\ y_N
\end{bmatrix}
=
\begin{bmatrix}
a_{11} & a_{12} & a_{13} & a_{14} & \cdots & a_{1d} \\
a_{21} & a_{22} & a_{23} & a_{24} & \cdots & a_{2d} \\
a_{31} & a_{32} & a_{33} & a_{34} & \cdots & a_{3d} \\
&& \vdots \\
a_{N1} & a_{N2} & a_{N3} & a_{N4} & \cdots & a_{Nd} \\
\end{bmatrix} \cdot 
\begin{bmatrix}
w_1 \\ w_2 \\ w_3 \\ w_4 \\ \vdots \\ w_d
\end{bmatrix}
$$

With an assumption that the label is determined with boundary zero, the positive parameter $(\textcolor{red}{1})$ means the neuron maps the activation to `label 1` and the negative parameter $(\textcolor{blue}{-1})$  means that the neuron maps the activation to `label 2`. Finally, the zero $(\textcolor{red}{0})$ means an invalid signal for classification. 

$$
\begin{bmatrix}
y_1 \\ y_2 \\ y_3 \\ \vdots \\ y_N
\end{bmatrix}
=
\begin{bmatrix}
a_{11} & a_{12} & a_{13} & a_{14} & \cdots & a_{1d} \\
a_{21} & a_{22} & a_{23} & a_{24} & \cdots & a_{2d} \\
a_{31} & a_{32} & a_{33} & a_{34} & \cdots & a_{3d} \\
&& \vdots \\
a_{N1} & a_{N2} & a_{N3} & a_{N4} & \cdots & a_{Nd} \\
\end{bmatrix} \cdot 
\begin{bmatrix}
\textcolor{red}{1} \\ \textcolor{blue}{-1} \\ \textcolor{green}{0} \\ w_4 \\ \vdots \\ w_d
\end{bmatrix}
$$

### Non-Linear 

The Non-linear function assumes that the activation of neuron could be used to distinguish the concept in non-linear manner. 
In the Othello paper, the authors used the function: 

$$f(x) = W_2 (\text{ReLU} (W_1))$$

## Intervention 

Let $F$ be a GPT model and $G$ be a classifier for linear or non-linear probing. 
The proposed intervention modifies the activation of the GPT to change the classifier output to the different class.
In Othello, let $x_t$ be the hidden representation at $t$-th token and $B$ is the current board state with $[x_1, \cdots, x_t]$. We have a desired board state $B'$ whose activation space are already known with classifier $G$. Then, we change the $x$ to the direction of the opponent label by gradient descent

$$ 
\begin{equation}
x' \leftarrow x - \alpha \frac{\partial \mathcal{L}_{CE}(G(x), B')}{\partial x}
\end{equation}
$$

There are additional details about the intervention.
1. The hidden representation is a key-vector
2. The intervened token is in the last time step of $L \times T$ block. 
3. When the middle layer is intervened, all the upper layers are changed while the layer below unchanged. 

> **What happens when middle layer is intervened:** <br> 
Let's assume that we have 8 layers and 5-th layer is intervened with Equation (1). After the activation is changed from $x_t^5$ to $(x^{'})_t^{5}$ to align a concept to $B'$, the next layer computes $(x^{'})_t^{6}$ with a transformer block. As the output may not align with the desired concept $B'$, it is further optimized with a classifier in the 6-th layer producing $(x^{"})_t^{6}$. The same procedure is applied to 7-th and 8-th layers.





## Latent Saliency Attribution 


The goal of LSA is to determine the contribution of previous tokens for the last prediction. That is, how the intervention of activations in the previous token will affect the logits  of the current token whether increase or decrease the likelihoods. 

<figure>
$$  
\underset{\Rightarrow \text{GPT Tokens}}{\begin{bmatrix}
\uparrow & \uparrow & \uparrow & \uparrow & \uparrow  & \uparrow & \uparrow & \uparrow & [L]  \\
\uparrow & \uparrow & \uparrow & \uparrow & \uparrow  & \uparrow & \uparrow & \uparrow & \uparrow  \\
\uparrow & \uparrow & \uparrow & \uparrow & \uparrow  & \textcolor{blue}{[x_s \rightarrow x_s']} & \uparrow & \uparrow & \uparrow  \\
\uparrow & \uparrow & \uparrow & \uparrow & \uparrow  & \uparrow & \uparrow & \uparrow & \uparrow  \\
\uparrow & \uparrow & \uparrow & \uparrow & \uparrow  & \uparrow & \uparrow & \uparrow & \uparrow 
\end{bmatrix}}
$$
<figcaption>
Description. LSA change the activation state $x_s$ of token $s$ to $x_s'$ by intervention and check the prediction of tokens from $L$, that is, $p_s$. The attribution is calculated by $p_o - p_s$ where $p_0$ is the prediction of $L$ without any intervention. 
</figcaption>
</figure>


### Interpret the sign of $p_0 - p_s$ :
* (➕) <strong style="color:red;"> Positive </strong> : As the probability decreased, the token was important for the prediction. 
* (➖) <strong style="color:blue;"> Negative </strong>: As the probability increased, the token was not important for the prediction. (Note that the intervention changes the state of a tile to other class.)


<figure>
<img src="/assets/side_articles/othello/lsa.png">
<figcaption markdown="1">
Figure. © Image from the [[paper](https://arxiv.org/abs/2210.13382)]. The bold solid square is the current prediction and the previous tokens are evaluated with LSA. 

</figcaption>

</figure>

## OthelloScope


1. 1️⃣ How does a neuron affects logits over 60 tokens?
2. 2️⃣ How do positions activate a neuron?
3. 3️⃣ How can a neuron be used to separate a concept?

<center>
<figure>
<img src="https://lh5.googleusercontent.com/_yTyJ0TweSeYEUleoGtbQhzXJzPD8_NEx9LBUblSBC90Uag8eGVou0ZnJuAMjM3dBaTzgqPPqePSQ6fwTGUHaqm1pHprc612GezxVGUpV3-Sh6n7ItZyq0ZA1e0blADnkt5ArGYkGFHd4P8ODgCo7AI">
<figcaption markdown="1">
Figure. 2️⃣  How do positions activate a neuron 1201 in layer 1. © Image from [[link](https://apartresearch.itch.io/othelloscope)]. <br> See [[OthelloScope](https://kran.ai/othelloscope/L1/N1201/index.html)].
</figcaption>
</figure>
</center>


<center>
<figure>
<img src="https://lh5.googleusercontent.com/4ZdRyOb4rKZsRLVE73ZGfCjHjQLAKp00LcJKri_lCLOwuEb299i01gGt_ku3dzIHQv9WF1Z2Iqhfhb5zOTGLVBvdvSCYLuT9kZcgLfNyrngvTcW8nVqCSbLf3OLJZ_TsuEZKQcEV5p7De0LEUcFOfuY">
<figcaption markdown="1">
Figure. (left and middle) 3️⃣ How can a neuron be used to separate a concept (left: my piece vs opponent, middle: black or not) (right) 1️⃣  logit lens for 60 tokens of  neuron 79 in layer 6. © Image from [[link](https://apartresearch.itch.io/othelloscope)]. See [[OthelloScope](https://kran.ai/othelloscope/L6/N79/index.html)].
</figcaption>
</figure>
</center>



## The Geometriy of Probes

* Points : weights of the linear probing
* Edges: if the tokens are neighboring.



### Rank by the variance of activations


