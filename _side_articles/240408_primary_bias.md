---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2024-04-07
featured: true
toc:
  - name: Introduction
    subsections:
      - name: Q&A
      - name: History
      - name: Contribution
  - name: Methods
    subsections:
      - name: Side Network
      - name: Cached Memory
      - name: Chunks
  - name: Experiments
    subsections:
      - name: Set Up
      - name: Long Context
      - name: Demonstrations
  - name: Notes

title: 'Primary Bias in RL 논문 리뷰 (ICML 2022)'
description: '이 포스팅은 The Primacy Bias in Deep Reinforcement Learning 논문에 대한 공부입니다.'
---


이 글은 [The Primacy Bias in Deep Reinforcement Learning](https://proceedings.mlr.press/v162/nikishin22a.html) <d-cite key="wang2024augmenting"> </d-cite> 논문에 대한 리뷰입니다. 


<h2 id="introduction"> 1. Introduction </h2>


<h3 id="q-a"> Pre-Questions and Answers  </h3> 

1. **Primary Bias의 정의는 무엇인가?** : 
2. **Primary Bias는 어떻게 측정하는가?** : 
3. **Primary Bias는 RL 알고리즘에 영향을 받는가?** : 
3. **Primary Bias는 어떻게 해소될 수 있는가?** : 
3. **Primary Bias에 대해서 learning rate을 키우는 경우 해소될 수 있는가?** : 


<h3 id="history"> Historical Notes </h3>

논문에 언급되어 있는 이전 연구에 대한 주요 흐름과 이 논문에서 해결하는 문제는 다음과 같다. 


<h3 id="contribution"> Main Contribution of Paper </h3>

1. 에이전트의 레이어를 periodically resetting 하여 primary bias를 제거하였다. (Atari 100k, DeepMind Control Suite)
2. 


##  Idea
<blockquote>
“Your assumptions are your windows on the world. Scrub
them off every once in a while, or the light won’t come in.” <br><br> -Issac Asimov
</blockquote>

둘 중에서 더 나은 에이전트는 누구일까? 

* Bob : 초반에는 잘못된 정보에 대해서 학습하고, 이후 깨끗한 데이터를 학습하여 리워드 100 도달 
* Alice : 깨끗한 데이터에 대해서 학습만 진행하여 리워드 100 도달


둘 다 깨끗한 데이터에 대해서 학습을 진행해도, Bob의 경우 이전에 배웠던 정보들이 머릿속 무의식에 남아있어 모델 성능을 저하시킬 수 있다 
<d-footnote> a cognitive bias demonstrated by studies of human learning (Marshall & Werder, 1972; Shteingart et al., 2013) </d-footnote>.

이는 초반에 학습한 품질이 낮은 데이터가 이후 학습에 영향을 준다는 점을 시사한다. 연속적인 데이터에 대해서 학습하는 RL이 지니는 primary bias를 탐구할 필요가 있다. 

<blockquote>
<strong> The Primacy Bias in Deep RL:</strong> a tendency to overfit early
experiences that damages the rest of the learning process.

</blockquote>



## Heavy Priming Causes Unrecoverable Overfitting 

<blockquote>
could overfitting on a single batch of early data
be enough to entirely disrupt an agent’s learning process?
</blockquote>

#### Setting 

1. Soft Actor Critic 
2. quadruped-run (DeepMind Control suite)
3. *heavy priming*: after collecting 100 data points, we update the agent $10^5$ times using the resulting replay buffer, before resuming standard training.




### Q2. is the data collected by an overfitted agent unusable for learning?

1. We train a SAC agent with 9 updates per step in the MDP: due to the primacy bias, this agent performs poorly. 
2. Then, we initialize the same agent from scratch but use the data collected by the previous SAC agent as its initial replay buffer

<blockquote>
The primacy bias is not a failure to collect proper data per se, but rather a failure to learn from it.
</blockquote>


## Experiments 

* for SPR, we reset only the final linear layer of the 5-layer Q-network over the course of training spaced 2 × 104 steps apart; 
* for SAC, we reset agent’s networks entirely every 2 × 105 steps since the networks have only 3 layers; 
* for DrQ, we reset the last 3 out of 7 layers of the policy and value networks 10 times over the course of training

[-] SPR: Data-efficient reinforcement learning with self-predictive representations (Schwarzer, M., 2020)


<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211418&authkey=%21ALgeXc8Ho_XyIs8&width=472&height=471" width="472" height="471" />
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211419&authkey=%21AHH_2zYpZjxZMvs&width=469&height=467" width="469" height="467" />
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211425&authkey=%21AAPekFXzwPdFmb8&width=929&height=252" width="929" height="252" />


#### Learning Dynamics 
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211428&authkey=%21AIjVdNASjbaRuCA&width=454&height=554" width="454" height="554" />



#### TD Learning 
Eπ [r(st, at) + γr(st+1, at+1) + · · · + γnQπ(st+n, at+n)]. Here, n controls a trade-off between the (statistical) bias of Qπ estimates and the variance of the sum of future rewards.

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211427&authkey=%21AJjziMeO_vpFNBk&width=927&height=235" width="927" height="235" />
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211426&authkey=%21AAoMcbXQedYLSuc&width=936&height=263" width="936" height="263" />


## TD Failure  

TD failure modes Temporal-difference learning, when
employed jointly with function approximation and offpolicy
training, is known to be potentially unstable (Sutton
& Barto, 2018). In sparse-reward environments,
the critic network might converge to a degenerate solution
because of bootstrapping mostly on it’s own outputs
(Kumar et al., 2020); having the non-zero reward
data might not sufficient to escape a collapse

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211431&authkey=%21ALmOeG40FmF0PwU&width=483&height=402" width="483" height="402" />


## Reset What and How

We conjecture that the difference lies in the degree of representation learning required for each domain: a significant chunk of knowledge in Atari is contained in the agent’s representations; it might be notably easier to learn features in DeepMind Control, especially when dealing with dense states.