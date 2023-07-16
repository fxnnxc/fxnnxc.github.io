---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-07-15
featured: true
img: /assets/side_articles/nmf/selection.png
toc:
  - name: Introduction
title: 'Non-negative Matrix Factorization (NMF)'
description: 'This is a gentle introduction of NMF. '
---


# Introduction 
Non-Negative matrix Factorization (NMF) is a kind of MF whose matrix is non-negative entries. NMF has been  widely used in the field of recommendation systems where users generate a massive number of information in the system and administrators wants to find the most important components are essential. Consider the following example to understand underlying assumptions. 

User 1 may select two items, user2 may select only the second item, and user 3 may select three times of the first item. 

* Item representations : $[1,0,0]$ and $[1,1,0]$
* User1 Representation : $[2,1,0] = [1,0,0] + [1,1,0]$
* User2 Representation : $[1,1,0] = 0\cdot [1,0,0] + [1,1,0]$
* User3 Representation : $[3,0,0] = 3\cdot[1,0,0] + 0 \cdot [1,1,0]$

When we stack the representations, we  observe the following matrix whose rows are user representations.

$$
\begin{bmatrix}
  2 & 1 & 0 \\
  1 & 1 & 0 \\
  3 & 0 & 0 \\
  \vdots & \vdots & \vdots  
\end{bmatrix}
=
\begin{bmatrix}
  - \text{user } 1 - \\
  - \text{user } 2 - \\
  - \text{user } 3 - \\
  \vdots 
\end{bmatrix}
$$

 

Now, our problem is the reverse problem of finding items by decomposing the matrix. In detail, we find **the underlying components** (items) and **weights which represent how the users select the components**. 

* User 1 : $$w_{11} * \mathbf{c}_1 + w_{12} * \mathbf{c}_2 + \cdots$$
* User 2 : $$w_{21} * \mathbf{c}_1 + w_{22} * \mathbf{c}_2 + \cdots$$  
* User 3 : $$w_{31} * \mathbf{c}_1 + w_{32} * \mathbf{c}_2 + \cdots$$  

> Mathematically, When we have a matrix of size `n` $\times$ `d` where `d` is the dimension of items and $n$ is the number of users, we may want to discover the underlying fixed number `k` of components.

In the simpler form, we have

$$
\begin{bmatrix}
  - \text{user } 1 - \\
  - \text{user } 2 - \\
  - \text{user } 3 - \\
  \vdots 
\end{bmatrix}_{n\times d}
= 
W \cdot H 
=
\begin{bmatrix}
  - \mathbf{w}_1 - \\
  - \mathbf{w}_2 - \\
  - \mathbf{w}_3 - \\
  \vdots 
\end{bmatrix}_{n\times k}
\cdot 
\begin{bmatrix}
  - \mathbf{c}_1 - \\
  - \mathbf{c}_2 - \\
  - \mathbf{c}_3 - \\
  \vdots 
\end{bmatrix}_{k\times d}
$$

### Note1 

The user1 representation is the linear combination of component representations with weights. The symbol $\textcolor{blue}{k}$ represents number of elements linear combined. 

$$
\begin{bmatrix}
  \textcolor{blue}{- \text{user } 1 -} \\
  - \text{user } 2 - \\
  - \text{user } 3 - \\
  \vdots 
\end{bmatrix}_{n\times d}
= 
\begin{bmatrix}
  \textcolor{blue}{- \mathbf{w}_1 -} \\
  - \mathbf{w}_2 - \\
  - \mathbf{w}_3 - \\
  \vdots 
\end{bmatrix}_{n\times \textcolor{blue}{k}}
\cdot 
\textcolor{blue}{\begin{bmatrix}
  - \mathbf{c}_1 - \\
  - \mathbf{c}_2 - \\
  - \mathbf{c}_3 - \\
  \vdots 
\end{bmatrix}}_{\textcolor{blue}{k}\times d}
$$

###  Note 2
There are many solutions to decompose the matrix into two factors $W$ and $H$. That is, the reverse problem does not guarantee the uniqueness of items and there are several ways we can decompose the matrix into weight matrix and component matrix. 


# Non-Negative Matrix Factorization (NMF)

Let $M \in \mathbb{R}^{n\times d }$  be a non-negative matrix and $k$ is the number of components that we obtain by factorizing the matrix $M$. The factorization results in two matrices, weights $W \in \mathbb{R}^{n\times k }$ and components $H \in \mathbb{R}^{k\times d }$. 

$$
M = W \cdot H
$$

According to Sklearn [[link](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.NMF.html)], the objective is not only recovering the matrix with $W$ and $H$, but also regularizing two matrices. 

$$ 
\begin{aligned}
L(W,H) &= 0.5  \Vert X -HW \Vert_{loss}^{2} \\
      &+ (L_1 \text{ regularization on vectors of } W) \\
      &+ (L_1 \text{ regularization on vectors of } H) \\
      &+ (\text{regularization on Frobenius norm of } W) \\
      &+ (\text{regularization on Frobenius norm of } H)   
\end{aligned}
$$



# NMF Toy Example


Let's consider a simple toy data for NMF. 
We have three four items: pizza, chicken, burger, and fries. However, the fires are always provided with a burger. Therefore, we have logically three items even though the representation is four dimensional vector. 


<img src="/assets/side_articles/nmf/components.png" style="width:100%">


* `Pizza` : $[1,0,0,0]$
* `Chicken` : $[0,1,0,0]$
* `Burger+Fries` : $[0,0,1,1]$


Now, users buy foods with choices are the Figure below. 

<center>
<img src="/assets/side_articles/nmf/selection.png" style="width:70%;">
</center>

As the users can buy multiple foods, we can further make a toy dataset with weights. 


* <span style="color:blue">blue color </span> : a person bought **two** packs
* <span style="color:red">red color </span>  : a person bought **three** packs

$$
M = 
\begin{bmatrix}
1 & 1 & \textcolor{blue}{2} & \textcolor{blue}{2} \\
1 & 0 & 0 & 0 \\
1 & 1 & 1 & 1 \\
1 & 0 & 1 & 1 \\
0 & 1 & \textcolor{red}{3} & \textcolor{red}{3} \\
1 & 1 & 0 & 0 \\
\end{bmatrix} 
$$

We can visualize the matrix with red colors.
<center>
<img src="/assets/side_articles/nmf/matrix.png" style="width:50%">
</center>



### NMF Result
When we run the NMF with Sklearn, we have the following result. 

<center>
<img src="/assets/side_articles/nmf/nmf2.png" style="width:100%">
</center>



We can clearly see that components are one-hot vectors, but the magnitudes of components are not equal to 1. In addition, every component has different magnitude unlike the designed items of our toy food designs. In addition, when we run the algorithm with different seed we have different results. 

### Run Again...
<center>
<img src="/assets/side_articles/nmf/nmf1.png" style="width:100%">
</center>

It suggests that the iterative optimization in Sklearn does not provide a unique solution for NMF. 
However, we conjecture that correlated features are coupled when matrix is factorized (see burger and fries locations.) We can clearly see that two entries are in the same component. 


# Discussion

There are several questions when I learn NMF.

1. Are all elements of weights and components matrices zero? [ T / F ]
2. In which matrix should be encourage sparsity [ W / H / Both ]
3. Why should we decrease the Frobenius norm? 
4. With permutation and scaling, all NMF results are similar? That is, how many distinct NMF results with permutation and scale invariance. 

