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
  - name: Idea
    subsections:
      - name: Heavy Priming
      - name: Primate Data  
  - name: Experiments
  - name: Conclusion

title: 'Primary Bias in RL 논문 리뷰 (ICML 2022)'
description: '이 포스팅은 The Primacy Bias in Deep Reinforcement Learning 논문에 대한 공부입니다.'
---


이 글은 [The Primacy Bias in Deep Reinforcement Learning](https://proceedings.mlr.press/v162/nikishin22a.html) <d-cite key="wang2024augmenting"> </d-cite> 논문에 대한 리뷰입니다. 


<h2 id="introduction"> 1. Introduction </h2>

강화학습 에이전트는 순처적으로 새로운 데이터를 마주하고 모델을 업데이트하여 더 나은 행동을 배운다. 지속적으로 모델을 업데이트하면서 기존 정보들은 잊거나 업데이트하며 새로운 정보들을 학습하여 행동을 업데이트 한다. 

아무 정보도 없는 사람이 배우는 것과 정보를 알고 있는 사람의 배우는 속도에 차이가 있음은 자명하다. 또한 잘못된 정보를 가지고 있다면, 이를 지우고 새로운 정보를 학습하는 것은 학습이 더욱 오래 걸릴 수 있다. 

마찬기지로 모델의 초기 파라미터에 대해서 특정 행동을 배우는 것과 기존 학습된 모델 파라미터에 대해서 새로운 행동을 배우는 것은 차이가 있다. 

이 연구에서는 초기에 학습한 정보에 대한 편향이 강화학습 에이전트의 이후 학습에 대해서도 큰 영향을 끼친다는 점을 탐구하였고, 특정 레이어에 대한 reset 방식이 모델 성능을 더욱 개선시킬 수 있다는 점을 보였다. 

<h3 id="q-a"> Pre-Questions and Answers  </h3> 

1. **Primary Bias의 정의는 무엇인가?** : 초반에 관찰한 데이터에 대한 학습이 이후 학습에 영향을 끼치는 것. 
2. **Primary Bias는 어떻게 측정하는가?** : 초기 데이터에 대해서 학습된 레이어를 리셋한 경우 학습 속도가 더욱 빠르다면 파라미터가 bias된 것이다. 
3. **Primary Bias는 RL 알고리즘에 영향을 받는가?** : 아니다. 다양한 알고리즘에 대해서 모두 reset을 해줘야 더욱 빠르게 학습됨을 확인하였다. 
4. **Primary Bias는 어떻게 해소될 수 있는가?** : 파라미터 리셋. 그러나 모델 구조 혹은 알고리즘마다 리셋을 하는 위치가 다르다. 또한 reset period는 경험적으로 찾아져야 한다. 
5. **Primary Bias에 대해서 learning rate을 키우는 경우 해소될 수 있는가?** : 아니다. learning rate이 크다고 해도 이미 학습된 표현을 바꿔야 하기에, 적절하지 못한 pre-trained parameter는 문제가 될 수 있다. 


<h3 id="history"> Historical Notes </h3>

* 주로 cognitive science에서 인지 관련하여 기존에 알고 있던 편향이 이후에도 의식 혹은 무의식적으로 남아있다는 것이 알려져있다. 

* Reset관련해서는 최근 연구된 Dormant Neuron (학습 과정에서 꺼지는 뉴런이 발생하는) 현상과 연관되어 있다. 

* 특정 레이어 (특히 마지막 레이어)를 리셋하는 것은 NLP 혹은 vision에서 마지막 레이어를 특정 테스크에 대해서 새롭게 학습하고 기존 feature extractor는 freeze하는 것과 유사하다. 

<h3 id="contribution"> Main Contribution of Paper </h3>

에이전트의 레이어를 periodically resetting 하여 primary bias를 제거하였다. (Atari 100k, DeepMind Control Suite)


##  Idea
<blockquote>
“Your assumptions are your windows on the world. Scrub
them off every once in a while, or the light won’t come in.” <br><br> -Issac Asimov
</blockquote>

둘 중에서 더 나은 에이전트는 누구일까? 

* 🍙 Bob : 초반에는 잘못된 정보에 대해서 학습하고, 이후 깨끗한 데이터를 학습하여 리워드 100 도달 
* 🌷 Alice : 깨끗한 데이터에 대해서 학습만 진행하여 리워드 100 도달

둘 다 깨끗한 데이터에 대해서 학습을 진행해도, Bob의 경우 이전에 배웠던 정보들이 머릿속 무의식에 남아있어 모델 성능을 저하시킬 수 있다 <d-footnote> a cognitive bias demonstrated by studies of human learning (Marshall & Werder, 1972; Shteingart et al., 2013) </d-footnote>.

이는 초반에 학습한 품질이 낮은 데이터가 이후 학습에 영향을 준다는 점을 시사한다. 연속적인 데이터에 대해서 학습하는 RL이 지니는 primary bias를 탐구할 필요가 있다. 

<blockquote>
<strong> The Primacy Bias in Deep RL:</strong> a tendency to overfit early
experiences that damages the rest of the learning process.
</blockquote>



### Heavy Priming

Heavy Priming Causes Unrecoverable Overfitting 

<blockquote>
Could overfitting on a single batch of early data
be enough to entirely disrupt an agent’s learning process?
</blockquote>

실험 세팅은 다음과 같다. 

1. Soft Actor Critic 
2. quadruped-run (DeepMind Control suite)
3. *heavy priming*: after collecting 100 data points, we update the agent $10^5$ times using the resulting replay buffer, before resuming standard training.
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211418&authkey=%21ALgeXc8Ho_XyIs8&width=472&height=471" width="472" height="471" />



### Primate Data  
Question: is the data collected by an overfitted agent unusable for learning?

1. We train a SAC agent with 9 updates per step in the MDP: due to the primacy bias, this agent performs poorly. 
2. Then, we initialize the same agent from scratch but use the data collected by the previous SAC agent as its initial replay buffer

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211419&authkey=%21AHH_2zYpZjxZMvs&width=469&height=467" width="469" height="467" />

<blockquote>
The primacy bias is not a failure to collect proper data per se, but rather a failure to learn from it.
</blockquote>



## Experiments 

* **for SPR**<d-footnote> Data-efficient reinforcement learning with self-predictive representations (Schwarzer, M., 2020)</d-footnote>, we reset only the final linear layer of the 5-layer Q-network over the course of training spaced 2 × 104 steps apart; 
* **for SAC**, we reset agent’s networks entirely every 2 × 105 steps since the networks have only 3 layers; 
* **for DrQ**, we reset the last 3 out of 7 layers of the policy and value networks 10 times over the course of training


<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211425&authkey=%21AAPekFXzwPdFmb8&width=929&height=252" width="929" height="252" />

### Learning Dynamics 
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211428&authkey=%21AIjVdNASjbaRuCA&width=454&height=554" width="454" height="554" />

### Replay Ratio 

Replay를 더욱 자주 하는 경우 모델의 파라미터는 더욱 초기 데이터에 편향적으로 편한다. 따라서 reset을 했을 때 성능향상이 크게 나타났다. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211427&authkey=%21AJjziMeO_vpFNBk&width=927&height=235" width="927" height="235" />

### TD Learning 

TD learning에서는 horizon에 대한 $n$ 값을 컨트롤하여 state-action value에 대한 추정을 진행한다. 만일 $n$이 작다면 variance가 작아지고, $n$이 크다면 variance가 커지고 bias는 작아지게 된다. 

$$
\mathbb{E}_\pi [r(s_t, a_t) + \gamma r(s_{t+1}, r_{t+1}), \cdots, \gamma^n Q_\pi(s_{t+n}, a_{t+n})]
$$

Here, $n$ controls a trade-off between the (statistical) bias of Qπ estimates and the variance of the sum of future rewards.

**Variance가 클수록 return 값은 더욱 들쭉날쭉이 되고, 초기 데이터에 더욱 편향적이다.**   파라미터 reset을 했을 때 성능향상이 컸다. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211426&authkey=%21AAoMcbXQedYLSuc&width=936&height=263" width="936" height="263" />


### TD Failure  

TD failure modes Temporal-difference learning, when
employed jointly with function approximation and offpolicy
training, is known to be potentially unstable (Sutton
& Barto, 2018). In sparse-reward environments,
the critic network might converge to a degenerate solution
because of bootstrapping mostly on it’s own outputs
(Kumar et al., 2020); having the non-zero reward
data might not sufficient to escape a collapse

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211431&authkey=%21ALmOeG40FmF0PwU&width=483&height=402" width="483" height="402" />


### Reset What and How

We conjecture that the difference lies in the degree of representation learning required for each domain: a significant chunk of knowledge in Atari is contained in the agent’s representations; it might be notably easier to learn features in DeepMind Control, especially when dealing with dense states.

## Conclusion

* 초기데이터에 너무 많은 learning을 하면 안된다. 
* catastrophic forgetting은 잘 일어나지 않을 수 있다. 
* 주기적으로 모델의 파라미터를 reset 해주자 

