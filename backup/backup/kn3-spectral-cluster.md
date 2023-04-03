---
layout: post
title: 'Spectral Clustering'
date: 2022-04-29 11:12:00-0400
description: 'Notations and concepts of spectral clustering'
tags: LinearAlgebra 
---


# Notations 

1. Similarity graph : $G = (V, E)$
2. A set of vertices :  $V = \set{v_1, v_2, \cdots, v_n}$
3. similarity between vertices : $s_{ij}$ = similarity between $v_i$ and $v_j$ 
4. Similarity matrix : $S = (s_{ij})$ 
5. Weigthed adjacency matrix of the graph $W = (w_{ij})_{i,j=1,2,\cdots, n}$
6. degree of vertex $v_i$ :  $d_i = \sum_{j=1}^n w_{ij}$
7. weight sum of two sets of vertices : $W(A,B) = \sum_{i\in A, j\in B} w_{ij}$
8. $\|A\|$ number of vertices in $A$ (number of nodes)
9. $vol(A) := \sum_{i \in A} d_i$ (number of edges)

<br/>
# Concepts 

## Construct Similarity graph from data 

Given $\set{x_1, \cdots, x_n}$

1. **$\epsilon-$neighborhood graph**  : $w_{ij} = \epsilon$ when $\|\|x_i - x_j\|\| \le \epsilon$
2. **$k-$nearest neighborhood graph**  :  $w_{ij}~ \mathrm{when} ~ (v_i \in \mathrm{neigh}_k(v_j)$) **or** $(v_j \in \mathrm{neigh}_k(v_i)$)
3. **mutual $k-$nearest neighborhood graph** : $w_{ij}~ \mathrm{when} ~ (v_i \in \mathrm{neigh}_k(v_j)$) **and** $(v_j \in \mathrm{neigh}_k(v_i)$)
4. **fully connected graph**  : $\exp{-\frac{\|\|x_i - x_j\|\|^2}{2\sigma^2}}$

*  (1), (2) and (3) set $w_{ij} = 0 $ when the edge is not connected

<br/>
## Laplace Matrix 

\begin{equation}
\Large{L = D - W}
\end{equation}


## Graph Cut perspective

\begin{equation}
\large{\mathrm{Cut} (A_1, \cdots, A_n) = \frac{1}{2} \sum_{i=1}^k W(A_i, \bar{A}_i)}
\end{equation}


<p>
\begin{equation}
\large{\mathrm{RatioCut}(A_1,\cdots, A_n)= \frac{1}{2} \sum_{i=1}^k 
\frac{W(A_i, \bar{A}_i)}{A_i}= \frac{1}{2} \sum_{i=1}^k \frac{Cut(A_i, \bar{A}_i)}{\textcolor{blue}{\|A_i\|}}}
\end{equation}
</p>

<p>
\begin{equation}
\large{\mathrm{NCut}(A_1, \cdots, A_n) = \frac{1}{2} \sum_{i=1}^k 
\frac{W(A_i, \bar{A}_i)}{vol(A_i)} = \frac{1}{2} \sum_{i=1}^k \frac{Cut(A_i, \bar{A}_i)}{\textcolor{blue}{vol(A_i)}}}
\end{equation}
</p>


<br/>

#### Objectives In Spectral Clustering 

* Obj 1 : minmize between cluster similarity $C(A, \bar{A})$
* Obj 2 : maximize with-in cluster similarity $W(A,A), W(\bar{A}, \bar{A})$

#### Relationship with Cuts and objectives
* NCut : normalized laplace matrix and use Obj1 and Obj2 
* RatioCut : unnormalized laplace matrix and use only Obj 1, that is minimize cluster similarity



## References 

* [A tutorial on spectral clustering](https://link.springer.com/article/10.1007/s11222-007-9033-z)
