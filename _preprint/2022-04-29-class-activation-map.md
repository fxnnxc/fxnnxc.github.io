---
layout: post
title: Class Activation Map and Grad CAM
date: 2022-04-29 16:11:00-0400
inline: false
---


# Notation 


1. Assume that there are activation of convolution layer of size $(K x W' x H'')$ : $F_k = A_{ij}^k$
2. Global Averagge Pooling makes  it to $M_k = \frac{1}{Z}\sum_{ij} A_{ij} $ where $Z = \sum_i \sum_j 1$
3. There are weights $C x K$ where  
4. $k$ is the index of a filter


## Defiinition (Class Activation Map) 


It is defined by 

\begin{equation}
CAM = \sum_{k} w_{ck}* A^{k}_{ij}
\end{equation}


## Definition (Gradient CAM)


\begin{equation}
Grad-CAM = \sum_{k}  a_{ck} * A^{k}_{ij}
\end{equation}