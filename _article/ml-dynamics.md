---
layout: post
title: 'Dynamics'
date: 2023-04-06 11:12:00-0400
description: None
tags: ML
category: ML
subcategory : Dynamics 
---




# Kalman Filter 

The Kalman filter is a recursive estimator. This means that only the estimated state from the previous time step and the current measurement are needed to compute the estimate for the current state. Notation $\hat{x}_{m\vert n}$ represents the estimate of $x$ at time $n$ given observations up to and including at time $m\le n$. 

### Given 


* Weights (most of time these weights are assumed to be constant):
    * $F_k$ : the state-transition model at time $k$
    * $H_k$ : the observation model at time $k$ 
    * $Q_k$ : hte covariance of the process noise at time $k$.  $w_k \sim \mathcal{N}(0, Q_k)$.
    * $R_k$ : hte covariance of the observation noise at time $k$.  $v_k \sim \mathcal{N}(0, R_k)$.
    * $B_k$ : the control-input model 

* Params:
    * $u_k$ : control
    * $w_k$ : noise of the dynamics model 
    * $v_k$ : noise of the observation model
    * $x_k = F_k x_{k-1} + B_k u_k + w_k$ : dynamics 
    * $z_k = H_k x_k + v_k$ : observation at time step $k$

<Blockquote style='background-color:#FFFFEE'>
<h1> Kalman Filter </h1>
<h3> Prediction </h3>
<li>  $\hat{x}_{k\vert k+1} = F_k x_{k-1 \vert k-1} + B_k u_k$ // state estimate mean  </li>  
<li>  $\hat{P}_{k\vert k+1} = F_k P_{k-1 \vert k-1}F_k^\top  + Q_k$  // estimate covariance matrix </li>

<h3> Update </h3>
<li> $\tilde{y}_k = z_k - H_k \hat{x}_{k \vert k-1}$ : measurement pre-fit residual (innovation) </li> 
<li> $S_k = H_k \hat{P}_{k\vert k-1} H_k^\top + R_k$ : pre-fit covariance (innovation) </li> 
<li> $K_k = \hat{P}_{k\vert k-1}H_k\top S_k^{-1}$    : <strong style='color:blue'>Optimal Kalman gain </strong> </li> 
<li> $x_{k \vert k} = \hat{x}_{k \vert k-1} + K_k \tilde{y}_k$ : <strong style='color:green'>Update state estimate </strong> </li>
<li> $x_{k\vert k} = (I- K_k H_k)\hat{x}_{k\vert k-1} + K_k z_k $ : <br> $\qquad \qquad$ a linear interpolation of estimated value and measurement with Kalman gain ($K_k$). </li>
<li> $P_{k\vert k } = (I - K_k H_k) \hat{P}_{k\vert k-1}$  : <strong style='color:green'>Update estimate covariance </strong> </li>
<li> $\tilde{y}_{k\vert k} = z_k - H_k \hat{x}_{k \vert k}$ : measurement post-fit residual (additional error after the update) </li>
<br>


</Blockquote>

## Bayesian Form 

* $p(x_k \vert x_{k-1}) = \mathcal{N}(F_kx_{k-1} Q_k)$
* $p(z_k \vert x_k) = \mathcal{N}(H_k x_{k-1} R_k)$ 
* $$p(x_{k-1} \vert Z_{k-1}) = \mathcal{N} (\hat{x}_{k-1}, P_{k-1})$$, where $Z_t = \{z_1, \cdots, z_t}$




---

# Reference 

[1] https://en.wikipedia.org/wiki/Kalman_filter