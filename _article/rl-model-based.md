---
layout: post
title: 'Model-based Reinforcement Learning '
date: 2023-04-15 11:12:00-0400
description: ""
tags: RL 
category: RL
subcategory : MBRL
---


# RL with Model

* Model-Free
    * **No Model🌏**
    * <span> <text style='color:green' >Learn</text> value function and policy🦾 from <text style='color:#E38D00' >real experience</text> </span>
* Model-Based RL 
    * Train a **model 🌏** from <text style='color:#E38D00' >real experience</text> 
    * <span> <text style='color:blue' >Plan </text>  value function and policy 🦾 from  <text style='color:#F60077' >simulated experience</text>   </span>
* Dyna 
    * Train a **model 🌏** from <text style='color:#E38D00' >real experience</text> 
    * <span> <text style='color:green' >Learn</text> and <text style='color:blue' >Plan </text> value function and policy 🦾 from  <text style='color:#F60077' >simulated experience</text>. </span> 




### Dyna-Q



<Blockquote>
<h3 style="margin:0px"> Dyna-Q Algorithm </h3>
<hr>
Algorithm parameters: step size  $\alpha \in (0 , 1] , \mathrm{discount~rate~} \gamma > 0$     <br> 
Initialize  $Q  ( s, a ) \mathrm{~and~} M( s, a ) \  \forall s \in S , a \in A ( s )$  <br>
<br>
1. $~~$Do forever:  <br>
2. $~~\quad$ $S\leftarrow $ current (nonterminal) state    <br>
3. $~~\quad$  $A \leftarrow \epsilon$-greedy $(S,Q)$ <br>
4. $~~\quad$ Execute action $A$; observe resultant reward, $R$, and state, $S'$    <br>
5. $~~\quad$ $Q(S,A) \leftarrow Q(S,A) + \alpha [R + \gamma \max_a Q(S', a) - Q(S,A)] $    <br>
6. $~~\quad$ $Model(S,A) \leftarrow R, S'$ (assuming deterministic environment)   <br>
7. $~~\quad$ Repeat $n$ times:   <text style="text-align: right;" > ($\triangleleft$  planning backup) </text> <br>
8. $~~\qquad$ $S\leftarrow $ random previously observed state    <br>
9. $~~\qquad$ $A\leftarrow $ action previously taken in $S$   <br>
10. $\qquad$ $R, S' \leftarrow Model(S,A)$    <br>
11. $\qquad$ $Q(S,A) \leftarrow Q(S,A) + \alpha [ R+ \gamma \max_a Q(S', a) - Q(S,A)] $ 
</Blockquote>


#### Dyna-Q+ 


**Dyna-Q with an exploration bonus that encourages exploration.**

This agent keeps track for each state-action pair of how many time steps
have elapsed since the pair was last tried in a real interaction with the environment.
The more time that has elapsed, the greater (we might presume)
the chance that the dynamics of this pair has changed and that the model of
it is incorrect

In particular, if the modeled reward for a transition is $R$, and the transition
has not been tried in $\tau$ time steps, then planning backups are done as if that
transition produced a reward of $R + \kappa \sqrt{\tau}$ for some small $\kappa$.


<Blockquote>
<h3 style="margin:0px"> Dyna-Q+ Algorithm </h3>
<hr>
Algorithm parameters: step size  $\alpha \in (0 , 1] , \mathrm{discount~rate~} \gamma > 0$, reward bonus $\textcolor{blue}{\kappa}$     <br> 
Initialize  $Q  ( s, a ),  M( s, a ), \mathrm{~and~} \textcolor{blue}{\tau (S,A)} \ , \forall s \in S , a \in A ( s )$  <br>
<br>
1. $~~$Do forever:  <br>
2. $~~\quad$ $S\leftarrow $ current (nonterminal) state    <br>
3. $~~\quad$  $A \leftarrow \epsilon$-greedy $(S,Q)$ <br>
4. $~~\quad$ Execute action $A$; observe resultant reward, $R$, and state, $S'$    <br>
5. $~~\quad$ $Q(S,A) \leftarrow Q(S,A) + \alpha [R + \gamma \max_a Q(S', a) - Q(S,A)] $    <br>
6. $~~\quad$ $Model(S,A) \leftarrow R, S'$ (assuming deterministic environment)   <br>
7. $~~\quad$ Repeat $n$ times:   <text style="text-align: right;" > ( $\triangleleft$ planning backup) </text>  <br>
8. $~~\qquad$ $S\leftarrow $ random previously observed state    <br>
9. $~~\qquad$ $A\leftarrow $ action previously taken in $S$   <br>
10. $\qquad$ $R, S' \leftarrow Model(S,A)$    <br>
11. $\qquad$ $Q(S,A) \leftarrow Q(S,A) + \alpha [ \textcolor{blue}{(R + \kappa \sqrt{\tau(S,A)})}+ \gamma \max_a Q(S', a) - Q(S,A)] $ 
</Blockquote>


### Full vs Sample Backups


* Back up values 
    * (First Axis) back up state values $v(s)$ or action values $q(s,a)$
    * (Second Axis) whether they estimate the value for the optimal policy $\pi_*$ or for an arbitrary given policy $\pi$

* Backup Methods
    * (*full* backup or *sample* backup) The other binary dimension is whether the backups are full backups, considering all possible events that might happen, or sample backups



|:-:|:-:|:-:|:-:|:-:|
|   | Full Backup |  Sample Backup| 
| $v_\pi(s)$  |  $V(s) \leftarrow    $  |
| $v_*(s)$  | $V(s) \leftarrow \max_a \sum_{s', r} p(s',r \| s,a) [r + \gamma V(s')]$ |
| $q_\pi(s,a)$  |$ Q(s,a) \leftarrow \sum_{s', r} \hat{p}(s',r \| s,a) [r(s,a) + \gamma V(s')]$ | 
| $q_*(s,a)$  | $ Q(s,a) \leftarrow \sum_{s', r} \hat{p}(s',r \| s,a) [r + \gamma \max_{a'} Q(s',a')]$ | 



--- 

# MBRL Survey 

Based on 

```
Luo, Fan-Ming, et al. "A survey on model-based reinforcement learning." arXiv preprint arXiv:2206.09328 (2022).
```



there is always a generalization error between the learned environment model and the real environment. 

Learning the model corresponds to recovering the state transition dynamics $M$ and the reward function $R$.


### Tabular MDP

$$
\hat{M}(s' | s,a) = 
\begin{cases}
\frac{C[s,a,s']}{\sum_{s'' \in S}C[s,a,s'']}, & \sum_{s'' \in \mathcal{S}} C[s,a,s''] >0  \\
\frac{1}{\mathcal{S}} & \text{otherwise}
\end{cases}
$$

$$
\hat{R}(s,a) = 
\begin{cases}
\frac{Sum[s,a]}{C[s,a]}, & C[s,a] > 0 \\
R_{min} & \text{otherwise}
\end{cases}
$$


### R-MAX 

R-max encourages exploration by setting the reward of an unexplored state and an action pair as the maximum value. 


$$
\hat{M}(s' | s,a) = 
\begin{cases}
\frac{C[s,a,s']}{\sum_{s'' \in S}C[s,a,s'']}, & \sum_{s'' \in \mathcal{S}} C[s,a,s''] >0  \\
I[s'=s] & \text{otherwise}
\end{cases}
$$

$$
\hat{R}(s,a) = 
\begin{cases}
\frac{Sum[s,a]}{C[s,a]}, & C[s,a] > 0 \\
R_{max} & \text{otherwise}
\end{cases}
$$

R-MAX requires 
$$
\mathcal{O}\Big( 
    \frac{\vert \mathcal{S}\vert^2 \vert\mathcal{A} \vert } { \epsilon^3 (1-\gamma)^2}\log \frac{1}{\delta}
    \Big)
$$
episodes to achieve a high accuracy ($\ell_1$ difference on transition $\le \epsilon/2$ )



# Mode Learning 

### Prediction Loss 

<Blockquote style='background-color:#FFFFEE'>
<h3> Simulation Lemma 1</h3>
Given an MDP with reward upper bound $R_{max}$ and transition model with $M^*$, and a data-collecting policy $\pi_D$, and a learned transition model $M_\theta$ with 
$$
\max_{s,a} \Vert M_\theta(s,a) - M^*(s,a) \Vert_1 \le \epsilon_m^{max} 
$$
and a learned reward function with 
$$
\max_{s,a} \vert R_\theta(s,a) - R(s,a) \vert \le \epsilon_r 
$$
, the value evaluation error of any policy $\pi$ is bounded as 



$$
\Vert 
V^\pi_{M_\theta} - V^\pi_{M^*} \Vert_\infty \le 
\frac{\gamma R_{max}}{(1-\gamma)^2} \epsilon_m^{max}
+ 
\frac{1}{1-\gamma} \sqrt{\epsilon_R}
$$


</Blockquote>


<Blockquote style='background-color:#FFFFEE'>
<h3> Simulation Lemma 2</h3>

Given an MDP with reward upper bound $R_{max}$ and transition model with $M^*$, and a data-collecting policy $\pi_D$, and a learned transition model $M_\theta$ with 
$$
\mathbb{E}_{(s,a) \sim \rho_{\pi_D}^{M^*}} \Big[ D_{KL}M^*(\cdot \vert s,a), M_\theta(\cdot \vert s, a)) \Big] \le \epsilon_m^{\rho}
$$
for an arbitrary policy $\pi$ with bounded divergence, 
$$
\max_s D_{KL} (\pi(\cdot|s), \pi_D(\cdot|s)) \le \epsilon_\pi
$$,
the policy evaluation error is bounded as 
$$
\vert 
V^\pi_{M_\theta} - V^\pi_{M^*} 
\vert\le 
\frac{\sqrt{2} R_{max}\gamma}{1-\gamma} \sqrt{\epsilon_m}
+ 
\frac{2\sqrt{2} R_{max}}{(1-\gamma)^2} \sqrt{\epsilon_\pi}
\vert
$$

</Blockquote>

### Distribution Matching 

GAIL method [Ho and Ermon, 2016] that imitates the expert policy in an adversarial manner, where a discriminator $\mathcal{D}$
learns to identity whether a state-action pair comes from the expert demonstrations and a generator $\pi$ imitates the expert
policy by maximizing the discriminator score.

Let $\rho_\pi$  bet the state-action distribution by running the policy $\pi$, and $\pi_E$  be the expert policy. When the discriminator is optimal to the inner objective, the generaotr essentially minimizes the Jensen-Shannon (JS) divergence between $\rho_{\pi_E}$ and $\rho_\pi$ 

$$
\min_{\pi \in \Pi} D_{JS}(\rho_{\pi_E}, \rho_\pi) := 
\frac{1}{2} \Big[
D_{KL}(\rho_{\pi_E}, \frac{\rho_\pi+\rho_{\pi_E}}{2})
+
D_{KL}(\rho_{\pi}, \frac{\rho_\pi+\rho_{\pi_E}}{2})
\Big]
$$


Let $\mu^{M}$ be the joint state-action-next-state distributions induced by the data-collecting policy $\pi_D$ in the true environment $M$. Formally,

$$
\mu^M(s,a,s') = \rho_{\pi_D}^M(s,a) M(s'\vert s,a )
$$
The optimal solution of $M_\theta$ minimizes the JS divergence between $\mu^{M^*}$ and $\mu^{M^\pi}$, matching the trajectory distribution. 

* Wu et al, (2019) chose to minimize the Wasserstein distance between $\mu^{M^*}$ and $\mu^{M^\pi}$. 
* [Wu el al., 2019, Wu et al., 2020] kept the data-collecting policy $\pi_D$ fixed during the training process of the transition model 
* [Shi et al., 2019] optimized the transition model and policy jointly.


<Blockquote style='background-color:#FFFFEE'>
<h3> Simulation Lemma 3 (Xu et al., 2020)</h3>
Given an MDP with reward upper bound $R_{max}$ and transition model with $M^*$, and a data-collecting policy $\pi_D$, and a learned transition model $M_\theta$ with 
$$
D_{JS}(\mu^{M_\theta}, \mu^{M^*}) \le \epsilon_m^{JS}
$$
for an arbitrary policy $\pi$ with bounded divergence, 
$$
\max_s D_{KL} (\pi(\cdot|s), \pi_D(\cdot|s)) \le \epsilon_pi
$$,
the policy evaluation error is bounded as 
$$
\vert 
V^\pi_{M_\theta} - V^\pi_{M^*} \vert \le 
\frac{2\sqrt{2} R_{max}}{1-\gamma} \sqrt{\epsilon_m^{JS}}
+ 
\frac{2\sqrt{2} R_{max}}{(1-\gamma)^2} \sqrt{\epsilon_\pi}

$$
remark that the coefficient on the model error $\epsilon^{JS}_m$ is linear w.r.t the effective horizon, .t.m $\frac{1}{1-\gamma}$.
<strong>The compounding error issue is solved </strong>
</Blockquote>


## Multi Step Prediction 

Since the compounding error is due to the recursive state-action generation using the one-step transition model, a way of alleviating the issue is to predict many steps at a time, A multistep model [Asadi et al., 2019] takes the current $s_t$ and a sequence of actions $(a_t, a_{t+1), \cdots, a_{t+h-1}$


$$
(s_{t+1}, \cdots, s_{t+h}) = M_\theta^h(s_t, a_{t}, \cdots, a_{t+h-1})
$$

This modeling can void the compounding error as there is no "fake" input. However, modeling multi-step is complex than single step and the error could be larger. 

## Mujoco Robot Locomotion 
The mainstream realization of the environment dynamics model is an ensemble of Gaussian processes where the
mean vector and covariance matrix for the distribution of the next state are built based on neural networks fed in the
current state-action pair [Chua et al., 2018]. Such an architecture is shown to work well on MuJuCo robot locomotion
environments, where the state observations are sufficient statistics for future derivation.
 

## Complex Environments Dynamics

* World Model [David Ha, 2018 NIPS] <text style='color:red'>[4] </text>: an autoencoder network to encode the latent state that can reconstruct the image.
* Deep Planning Network (PlaNet) [Hafner et al., 2019a] : Gaussian latent
* DreamerV1 [Hafner et al. [2020]] : the latent dynamics for visual control tasks, in which an environment model (called world model) with visual encoder and latent dynamics is learned based on collected experience
* DreamerV2 [Hafner et al. [2021]]: Discrete latent. the discrete latent representation can better fit the aggregate posterior and handle multi-modal cases.
* IRIS 

---

# Planning

### Model Predictive Control 

### Monte Carlo tree search (MCTS)


# References 

[1] Sutton, Richard S., and Andrew G. Barto. Reinforcement learning: An introduction. MIT press, 2018.


[2] RL Course by David Silver - Lecture 8: Integrating Learning and Planning ([Youtube](https://www.youtube.com/watch?v=ItMutbeOHtc))

[3] Generative Adversarial Imitation Learning  [Ho and Ermon,2016] 


[4] Recurrent World Models Facilitate Policy Evaluation