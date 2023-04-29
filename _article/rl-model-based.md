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



<Blockquote style='background-color:#F0FFEF'>
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


<Blockquote style='background-color:#F0FFEF'>
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



<h1 style="font-family:Georgia; color:#009900; padding-top:100px">  MBRL Survey  </h1>
<hr>

Model-Based Reinforcement Learning (MBRL) Based on 

<Blockquote style="border:2px solid; background-color:#DDEEDD;">
Luo, Fan-Ming, et al. "A survey on model-based reinforcement learning." arXiv preprint arXiv:2206.09328 (2022).
</Blockquote>



there is always a generalization error between the learned environment model $M_\theta$ and the real environment $M^\*$. Learning the model corresponds to recovering the state transition dynamics $M^\*$ and the reward function $R^\*$.


<h2 style="font-family:Georgia; color:#000099; padding-top:50px"> 1. Tabular MDP  </h2>
<hr>

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


### 1.1 R-MAX 

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


<h2 style="font-family:Georgia; color:#000099; padding-top:50px"> 2. Model Learning  </h2>
<hr>

### 2.1 Prediction Loss 

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


### 2.2 Transition Distribution Matching 

<Blockquote style='background-color:#FFFFEE'>
<h3> Simulation Lemma 2</h3>

Given an MDP with reward upper bound $R_{max}$ and transition model with $M^*$, and a data-collecting policy $\pi_D$, and a learned transition model $M_\theta$ with 
$$
\mathbb{E}_{(s,a) \sim \rho_{\pi_D}^{M^*}} \Big[ D_{KL}(M^*(\cdot \vert s,a), M_\theta(\cdot \vert s, a)) \Big] \le \epsilon_m^{\rho}
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

### 2.3 Trajectory Distribution Matching 

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
\max_s D_{KL} (\pi(\cdot|s), \pi_D(\cdot|s)) \le \epsilon_\pi
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
<br>

<strong>📌 The compounding error issue is solved </strong>
</Blockquote>


### 2.4 Mutli-Step Prediction

Since the compounding error is due to the recursive state-action generation using the one-step transition model, a way of alleviating the issue is to predict many steps at a time, A multistep model [Asadi et al., 2019] takes the current $s_t$ and a sequence of actions $(a_t, a_{t+1), \cdots, a_{t+h-1}$


$$
(s_{t+1}, \cdots, s_{t+h}) = M_\theta^h(s_t, a_{t}, \cdots, a_{t+h-1})
$$

This modeling can void the compounding error as there is no "fake" input. However, modeling multi-step is complex than single step and the error could be larger. 

<h2 style="font-family:Georgia; color:#000099; padding-top:50px"> 3. Complex Environment Modeling  </h2>
<hr>


### 3.1 Mujoco Robot Locomotion 
The mainstream realization of the environment dynamics model is an ensemble of Gaussian processes where the
mean vector and covariance matrix for the distribution of the next state are built based on neural networks fed in the
current state-action pair [Chua et al., 2018]. Such an architecture is shown to work well on MuJuCo robot locomotion
environments, where the state observations are sufficient statistics for future derivation.
 

### 3.2 Complex Environments Dynamics

* **☕️ World Model** [David Ha, 2018 NIPS] <text style='color:red'>[4] </text>: an autoencoder network to encode the latent state that can reconstruct the image.
* **☕️ Deep Planning Network (PlaNet)** [Hafner et al., 2019a] : Gaussian latent
* **☕️ DreamerV1** [Hafner et al. [2020]] : the latent dynamics for visual control tasks, in which an environment model (called world model) with visual encoder and latent dynamics is learned based on collected experience
* **☕️ DreamerV2** [Hafner et al. [2021]]: Discrete latent. the discrete latent representation can better fit the aggregate posterior and handle multi-modal cases.
* **☕️ IRIS** (Imagination with auto-Regression over an Inner Speech)

---

<h2 style="font-family:Georgia; color:#000099; padding-top:50px"> 4. Planning  </h2>
<hr>

### 4.1 Model Predictive Control 

MPC [Camacho and Alba, 2013] is a kind of model-based control method that plans
an optimized sequence of actions in the model.


* Classic
*  Learning Based : Learning-based MPC [Hewing et al., 2020] has a tight connection with MBRL. In general, at each time step, MPC obtains an optimal action sequence by sampling multiple sequences and applying the first action of the sequence to the environment.

$$
\Large{\max_{a_{t:t+\tau}} \mathbb{E}_{s_{k+1} \sim p(s_{k+1}|s_{k}, a_{k})} \Big[  \sum_{k=t}^{t+\tau} r(s_k, a_k) \Big]}
$$

where $\tau$ denotes the planning horizon. Then the agent will choose the first action $a_t$ from the action sequence and apply it to the environment. 

<Blockquote style='background-color:#002200; width:120%; margin-left:-100px'>
<h3 style='color:#FFFFFF;'> Monte Carlo (MC) method (also known as “random shooting”), </h3>
<li style='color:#FFFFFF;'><strong style='color:#BBBBBB;'>Step1:</strong>  samples a number of action sequences $a_{t:t_\tau}$ from the space of action sequence uniformly and randomly.  </li>
<li style='color:#FFFFFF;'><strong style='color:#BBBBCC;'>Step2:</strong>  Applying the action sequences in the model, the current state $s_t$ can be transited to $s_{t+\tau}$ following the transition distribution.  </li>
<li style='color:#FFFFFF;'><strong style='color:#BBBBEE;'>Step3:</strong>  Evaluate the action sequences with the accumulated returns.  </li>
<li style='color:#FFFFFF;'><strong style='color:#BBBBFF;'>Step4:</strong>  Choose the action sequence with the highest return.  </li>
</Blockquote>



Recent advances in MPC methods focus on altering **the sampling strategies** [Chua et al., 2018, Hafner et al., 2019b] and the sampling space [Wang and Ba, 2020]. Replacing the MC method with CEM [Botev et al., 2013], PETS [Chua et al., 2018], and PlaNet [Hafner et al., 2019b]
improves the optimization efficiency.

* **CEM**: samples the action sequences from a multivariate normal distribution, which will be adjusted according to the evaluation of the sampled sequences. 
* **POPLIN-A** [Wang and Ba, 2020] further improves the optimization efficiency by altering the
sampling space to a space of action bias. Specifically, POPLIN-A seeks a sequence of action residual to adjust an action sequence proposed by a policy.
* **I2A** [Racanière et al., 2017] integrates Monte Carlo planning results into model-free RL framework as the auxiliary information. Instead of applying the first action of the sequence with the highest return, I2A encodes several rollouts from the model to rollout embeddings. The embeddings will then be aggregated and used to augment the input of the model-free RL agent.


### 4.2 Monte Carlo tree search (MCTS)

MCTS adopts a tree-search method.

<Blockquote style='background-color:#000022; width:120%; margin-left:-100px'>
<h3 style='color:#FFFFFF;'> Monte Carlo Tree Search (MCTS) method </h3>
<li style='color:#FFFFFF;'> <strong style='color:#BBBBFF;'>Step1:</strong> MCTS incrementally extends a search tree from the current environment state </li>
<li style='color:#FFFFFF;'> <strong style='color:#BBFFBB;'>Step2 (option 1):</strong> Each node in the tree corresponds to a state, which will be evaluated by some approximated value functions </li>
<li style='color:#FFFFFF;'> <strong style='color:#BBFFBB;'>Step2 (option 2):</strong> or the return obtained after rollouts in the model with a random policy or a neural network policy.  </li>
<li style='color:#FFFFFF;'> <strong style='color:#FFBBBB;'>Step3:</strong> Finally, action will be chosen such that the agent can be more likely transited to a state which has a higher evaluated value. </li>
<text style='color:#FFFFFF'> In MCTS, models are generally used to generate the search tree and evaluate the state.</text>
</Blockquote>


* **AlphaGo** [Silver et al., 2016:  For each decision timestep, AlphaGo uses MCTS to decide where to play the next stone on board.
* **AlphaGo Zero** [Silver et al., 2017b] : trains a policy to mimic the planning outputs of MCTS, like POPLIN-A. The policy is then used to generate the search tree in MCTS. The procedure of AlphaGo Zero can be regarded as an iteration between improving a policy by planning and enhancing the planning by the policy. 
* **Value prediction network (VPN)** [Oh et al., 2017b] learns an abstract state transition model. VPN applies MCTS to the learned model to search an action sequence that has the highest bootstrapped environment return.
* **MuZero** [Schrittwieser et al., 2019] : learns a transition model but additionally learns an abstract policy, which outputs actions with the abstract states as inputs.

# References 

[1] Sutton, Richard S., and Andrew G. Barto. Reinforcement learning: An introduction. MIT press, 2018.


[2] RL Course by David Silver - Lecture 8: Integrating Learning and Planning ([Youtube](https://www.youtube.com/watch?v=ItMutbeOHtc))

[3] Generative Adversarial Imitation Learning  [Ho and Ermon,2016] 


[4] Recurrent World Models Facilitate Policy Evaluation