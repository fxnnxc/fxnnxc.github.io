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


<hr>

# World Model 


* World Model 
* Dreamer 
* Dreamer2


<hr>



--- 

# Survey 

there is always a generalization error between the learned environment model and the real environment. 

Learning the model corresponds to recovering the state transition dynamics $M$ and the reward function $R$.


### Tabular MDP

$$
\hat{M}(s' | s,a) = 
\begin{cases}
\frac{C[s,a,s']}{\sum_{s'' \in S}C[s,a,s'']}, & \sum_{s'' \in \mathcal{S}}} C[s,a,s''] >0  \\
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
\frac{C[s,a,s']}{\sum_{s'' \in S}C[s,a,s'']}, & \sum_{s'' \in \mathcal{S}}} C[s,a,s''] >0  \\
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

R0MAX requires 

$$
\mathcal{O}\Big( 
    \frac{\vert \mathcal{S}\vert^2 \vert\mathcal{A} \vert } { \epsilon^3 (1-\gamma)^2}\log \frac{1}{\delta}
    \Big)
$$
episodes to achieve a high accuracy ($\ell_1$ difference on transition $\le \epsilon/2$ )



### Model Learning by Distribution Matching 

GAIL method [Ho and Ermon,2016]

Discriminator learns to identify whether a state-action pair comes from the expert demonstrations and a generator $\pi$ imitates the expert policy by maximizing the discriminator score. 


### Robust Model Learning 



## Planning

### Model Predictive Control 

### Monte Carlo tree search (MCTS)


# References 

[1] Sutton, Richard S., and Andrew G. Barto. Reinforcement learning: An introduction. MIT press, 2018.


[2] RL Course by David Silver - Lecture 8: Integrating Learning and Planning ([Youtube](https://www.youtube.com/watch?v=ItMutbeOHtc))
