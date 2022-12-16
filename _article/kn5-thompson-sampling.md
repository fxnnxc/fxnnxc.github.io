---
layout: post
title: 'Thomson Sampling'
date: 2022-05-08 00:00:00-0400
description: 'Notations and concepts of spectral clustering'
tags: Stats, RL
---

## Abstract

Given model parameters $\theta_1, \cdots, \theta_n$ and prior distribuiton over $\theta$, compute the posterior distribution and obtain sample $\theta^*$.  Executing action 
$a = \arg \max_a \mathbb{E}[r| \theta^\*, a, x ]$
. Prior is updated again. 

1. sample model parameters 
$\theta_1^\*, \theta_2^\*, \cdots, \theta_n^\*$. 
2. Choose the best action and model parameter $\theta_k$
3. Execute an action and update the posterior distribution of $\theta_k$ 

## Multi-Armed Bandit

* Win or Lose after selecting an arm -> bernoulli distirubion 
* probability of winning is beta distiribution. 

All the model parameter is Beta distribution with $\alpha=1, \beta=1$.
For each model parameter $\theta_i$, the posterior distribution is computed by 

$$
\mathcal{B}(\alpha, \beta) = \begin{cases}
\mathcal{B}(\alpha+1, \beta)  & (win) \\
\mathcal{B}(\alpha, \beta+1)  & (lose)
\end{cases}
$$





### Questions 

1. Selection is based on reward, do we pick on at each iteration and update the posterior only for the element?




