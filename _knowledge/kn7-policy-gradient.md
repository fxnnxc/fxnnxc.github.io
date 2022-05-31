---
layout: post
title: '#7 Policy Gradient'
date: 2022-05-31 00:00:00-0400
description: 'policy gradient methods'
tags: RL 
---



Formula $\nabla_\theta J(\theta)$ 



### 1. Reinforce 

 $$\Large{\mathbb{E}_{\pi_\theta}[\nabla_\theta \log \pi_\theta (s,a) V_t]}$$
 
 * parameterized model: $\pi_\theta(s,a)$   
 * computed by the episode: $V_t$ 

<hr/>

### 2.  Q Actor Critic (Action-value actor critic)
  
$$\Large{\mathbb{E}_{\pi_\theta}[\nabla_\theta \log \pi_\theta (s,a) Q^w(s,a)]}$$
 
 * parameterized model: $\pi_\theta(s,a)$ , $Q^w(s,a)$  
 * computed by the episode:  x
 * How to train Q : TD error $r_t + \gamma Q(s_{t+1}, a_{t+1)}) - Q(s_t, a_t) $

<hr/>

### 3. Advantage Actor Critic

$$\Large{\mathbb{E}_{\pi_\theta}[\nabla_\theta \log \pi_\theta (s,a) A^w(s,a)]}$$
 
with $A^w(s,a) = Q^w(s,a) - V^u(s)$

However, $Q(s_t, a_t) = \mathbb{E}[r_{t+1} + \gamma V^u(s_{t+1})]$, hence $A(s_t, a_t) = r_{t+1} + \gamma V^u(s_{t+1}) - V^u(s_t)$

Therefore, use one network for value function.


 * parameterized model: $\pi_\theta(s,a)$ , $V^u(s)$  
 * computed by the episode:  x
 * How to train V : TD error $r_t + \gamma V^u(s_{t+1}) - V^u(s_t, a_t)$ with stop gradient on $\gamma V^u(s_{t+1})$ (not sure).




### 4. TD Actor Critic





### Reference 

https://julien-vitay.net/deeprl/ActorCritic.html