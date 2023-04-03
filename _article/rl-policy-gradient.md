---
layout: post
title: 'Policy Gradient'
date: 2022-05-31 00:00:00-0400
description: 'policy gradient methods'
tags: RL 
category: RL
subcategory : Policy 
---

General formula maximizes <span> $$\nabla_\theta J(\theta) = \mathbb{E}_{s \sim \rho^\pi, a \sim \pi_\theta}[\nabla_\theta \log \pi_\theta (s,a) \Psi ]$$</span>


### Policy Gradient Utilities ($\Psi$)

🏃‍♂️(episodic value, sample),  🤖(neural net, approximataion), 🔮(expectation, oracle)

1. 🏃‍♂️ $V_t = \sum_{t'=t}^T \gamma^{t'-t} r(s_{t'} a_{t'}, s_{t'+1}$) : episode return 
2. 🤖 $Q_\theta(s,a)$ : action value (more stable than episode return ) 
3. 🔮 $A^\pi(s,a) = Q^\pi(s,a) - V^\pi(s)$ : 
    * with Advantage estimate $V_\psi(s)$,  <span style="color:green">$Q^\pi(s,a)$</span> can be replaced by 

    1. <span style="color:green">$R(s,a)$</span>$- V_\psi(s)$ : MC advantage estimate  🏃‍♂️ /  🤖
    2. <span style="color:green">$r(s,a,s') + \gamma V_\psi(s)$</span> $- V_\psi(s)(s)$ : TD advantage estimate 🤖 /  🤖
    3. <span style="color:green">$(\sum_{k=0}^{n-1} \gamma^k r_{t+k+1} +\gamma^n  V_\psi(s_{t+n+1}))$</span> $- V_\psi(s)(s)$ : TD advantage estimate 🤖 /  🤖


### 1. Reinforce 


<p align="left">
Maximize <span style="color:navy">$\mathbb{E}_{\pi_\theta}[\nabla_\theta \log \pi_\theta (s,a) V_t]$</span>
 </p>

 * parameterized model: $\pi_\theta(s,a)$   
 * computed by the episode: $V_t$ 

<hr/>

### 2.  Q Actor Critic (Action-value actor critic)
  

<p align="left">
Maximize <span style="color:navy">$\mathbb{E}_{\pi_\theta}[\nabla_\theta \log \pi_\theta (s,a) Q_\theta(s,a)]$</span>
 </p>


 * parameterized model: $\pi_\theta(s,a)$ , $Q_\theta(s,a)$  
 * computed by the episode (MC):  $\hat{Q}(s_t,a_t) =\sum_{t'=t}^T \gamma^{t'-t} r(s_{t'}, a_{t'}, s_{t' +1}) $
 * How to train Q : Approximated Q values and true Q-values $(\hat{Q}(s, a) - Q_\theta(s, a))^2$

<hr/>

### 3. Advantage Actor Critic


<p align="left">
Maximize <span style="color:navy">$\mathbb{E}_{\pi_\theta}[\nabla_\theta \log \pi_\theta (s,a) A_\psi(s,a)]$</span>
 </p>

 
with $A^\pi(s,a) = Q^\pi(s,a) - V^\pi(s)$ can be approximated by  $A_\psi(s,a)$

1. <span style="color:green">$R(s,a)$</span>$- V_\psi(s)$ : MC advantage estimate  🏃‍♂️ /  🤖
2. <span style="color:green">$r(s,a,s') + \gamma V_\psi(s)$</span> $- V_\psi(s)(s)$ : TD advantage estimate 🤖 /  🤖
3. <span style="color:green">$(\sum_{k=0}^{n-1} \gamma^k r_{t+k+1} +\gamma^n  V_\psi(s_{t+n+1}))$</span> $- V_\psi(s)(s)$ : TD advantage estimate 🤖 /  🤖

The reasons are,

1. $Q^{\pi}(s,a) = \mathbb{E}_{s\sim p^\pi, a \sim \pi}[R(s,a)]$
2. <p>$Q^\pi(s_t, a_t) = \mathbb{E}_{s\sim p^\pi, a \sim \pi}[r_{t+1} + \gamma V^\pi(s_{t+1})]$</p>
3. <p>$Q^\pi(s_t, a_t) =\mathbb{E}_{s\sim p^\pi, a \sim \pi}[\sum_{k=0}^{n-1} \gamma^k r_{t+k+1} +\gamma^n  V^\pi(s_{t+n+1})]$</p>

 * parameterized model: $\pi_\theta(s,a)$ , $V^u(s)$  
 * How to train V : TD error $r_t + \gamma V_\psi(s_{t+1}) - V_\psi(s_t, a_t)$ with stop gradient on $V_\psi(s_{t+1})$ (not sure).


### Reference 

https://julien-vitay.net/deeprl/ActorCritic.html