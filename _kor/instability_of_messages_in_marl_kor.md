---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2023-08-12
featured: true
img: /assets/side_papers/subjectivitiy_information/cartpole_messages.png
title: 'Investigating Instability of Messages in Multi-agent Reinforcement Learning [한국어]'
description: 'Analysis of stability of hidden representation messages in multi-agent.'
tags: MARL RL RepresentationLearning
---


# 국문체 

로봇이 움직이면서 상호작용하는 환경은 복잡한 공간이다. 이는 환경이 스스로 변하는 이유도 있지만 환경 내부의 다른 로봇들이 예상치 못하는 행동을 할 수 있기 때문이다. 이러한 다른 로봇들의 행동까지 고려해서 로봇을 심층학습으로 학습시키는 대표적인 분야는 다중에이전트 강화학습(MARL, multi-agent reinforcement learning)으로 여러 대의 로봇들의 상호작용을 강화학습을 통해서 배우는 분야이다. 
다중에이전트 강화학습은 여러 형태가 있지만, 그 중에서 커뮤케이션 기반 학습은 로봇들끼리 의사소통을 통하여 정보 교환을 하고 환경에서 최대보상을 이끌어낸다. 이는 개별 에이전트가 환경으로부터 얻는 정보는 국소적이며 여러 대의 로봇들이 센서를 통해서 입력받은 정보들을 종합하여 개별로봇의 의사결정을 돕는 것이다. 커뮤니케이션 기반 학습은 로봇이 다른 로봇에게 메시지를 전달하는데, 이 때 생성된 메시지는 신경망으로부터 로봇이 관찰한 값을 인코딩한 잠재표현이다. 이 메시지는 로봇의 신경망으로부터 가공되서 전달하므로 학습이 제대로 됐다면, 수신하는 로봇의 입장에서 필요한 정보가 담기게 된다. 

신경망을 기반으로 메시지를 만들면 필요한 메시지를 만들 수 있다는 사실이 보장되지만,  
메시지가 학습과정에서는 불안정한 신경망으로부터 생기므로, 메시지의 질적인 부분에서 하자가 있을 수 있다. 
또한 로봇이 관찰한 센서값들은 객관적인데 반해서 신경망으로부터 파생된 정보는 보다 주관적이다. 왜냐하면 동일한 센서로부터 서로 다른 두 신경망은 다른 잠재표현을 만들기 때문이다.


## 심층강화학습 

강화학습은 환경에서 에이전트가 주어진 상태에 대해서 행동을 결정하고, 행동에 대한 보상을 최대화하는 학습이다. 
심층강화학습에서는 에이전트가 상태에 대해서 행동을 결정하기 위해서 심층신경망을 사용하며, 주어진 행동으로부터 받는 에피소드 내 평균보상을 최대화하는 방식으로 학습된다. 
심층 강화학습의 알고리즘으로는 DQN, PPO, SAC과 같은 알고리즘들이 대표적으로 사용된다.  

## 다중에이전트 강화학습

심층강화학습이 하나의 에이전트 행동을 학습하는 것이 목표였다면, 다중에이전트 강화학습에서는 여러 에이전트들을 동시에 학습하여 사용자가 설계한 전체보상함수를 최대화하도록 학습된다. 이 때 에이전트들은 서로 돕거나 방해할 수 있으므로 에이전트들간의 추가적인 상호작용을 고려해야 하는 학습 방법이다. 
에이전트들을 학습하는 방식은 에이전트들의 정보교환의 정도에 따라서 중앙집중형과 분산형으로 구분될 수 있는데, 가장 많이 사용되는 방식은 학습 때는 중앙집중형으로 학습하고 테스트 때는 분산형으로 진행하는 (Centralized Training and Decentralized Execution) 방식이다. 이 방식의 대표적인 알고리즘들은 Q-MIX <d-cite key="rashid2018qmix"/> 와 MADDPG<d-cite key="lowe2017multi"/>가 있다.  이들은 에이전트들간의 커뮤니케이션이 존재하진 않으므로 행동을 결정할 때는 에이전트가 본인의 관찰값만 사용하지만, 보상에 대한 정확한 예측을 위해서 종합된 정보를 사용한다는 특징이 있다. 이와는 반대로 행동을 결정할 때 에이전트들간의 커뮤니케이션을 하는 방법은 대표적으로 두 가지가 있다. 
TarMAC <d-cite key="das2019tarmac"/> 은 에이전트들이 메시지를 보내고 받은 메시지를 종합적으로 고려하여 행동하는 방식이다. 그러나,TarMAC은 메시지가 항상 전달된다는 단점이 있는데, 이를 개선하여 요청-발신 구조를 제안한 I2C<d-cite key="ding2020learning"/>가 있다. 
두 커뮤니케이션 기반 에이전트들의 학습은 공통적으로 에이전트의 메시지가 신경망으로부터 생성되어 다른 에이전트로 전달되는 특징이 존재한다. 학습 과정에서 신경망은 지속적으로 변하며 전달하는 메시지 또한 동일한 관찰에 대해서 계속 달라질 수 있다. 이러한 전달하는 값에 대한 불안정성은 강화학습을 더욱 불안정하게 만들 수 있으며, 학습을 위해서 더 많은 시간이 필요하다. 이를 개선하기 위해서는 신경망으로부터 처리된 정보뿐만 아니라, 보다 에러가 적은 정보인 신경망의 입력 또한 에이전트에게 메시지로 전달해야 한다. 

## 정보의 주관성

신경망의 표현값에 대한 일반적인 믿음은 심층값이 입력에 대한 축약된 정보라는 것이다. 그러므로 이러한 정보는 단순히 입력에 대한 중요정보로 고려될 수 있으므로 주관적이라고 보기 어렵다. 그러나, 최근 딥러닝 해석 (Interpretability) 연구들은 모델의 내부에 대한 해석을 가능하게 만들었고, 모델 내부가 입력에 대한 정보를 처리하여 보관한다는 것이 알려졌다 <d-cite key="olah2020zoom"/><d-cite key="cammarata2021curve"/>. 또한, 이러한 보관방식은 모든 모델이 똑같다고 보기는 어려운데, 학습의 방법, 모델 초기화에 따라서 다르게 표현공간이 학습되기 때문이다. 따라서 모델마다 정보를 처리하는 방식이 서로 다를 가능성이 존재한다. 따라서, 신경망은 정보를 서로 다르게 처리하므로 객과적이기보다 주관적이라고 보는 시선이 올바르다. 


# 제안하는 알고리즘 

기존 다중에이전트 알고리즘들은 에이전트간의 통신하는 방식에 대해서 논하면서, 전달 하는 메시지는 신경망으로부터 생성된 출력이었다. 생성된 출력은 입력에 대한 정보를 담을 수 있지만, 정보처리 관점에서 신경망이 깊어질수록 입력의 정보는 희석된다. 본 논문에서 제안하는 방법론은 입력 또한 중요한 정보이며, 때로는 신경망으로부터 생성된 정보보다 더욱 안정적으로 다중에이전트학습에 도움이 될 수 있다는 것이다. 그림1은 신경망이 깊어질수록 주관적인 정보가 점차 증가한다는 것을 보이며, 제안하는 방법은 단순히 신경망의 출력뿐만아니라 처리과정을 줘야 한다는 것이다. 

<figure style="text-align:center">
<img align src="/assets/side_papers/subjectivitiy_information/subjectivity.png" style="width:100%">
<figcaption>
그림 1. 두 에이전트가 통신하는데 있어서 메시지를 전달하는 방식 비교. (좌) 신경망 기반 메시지는 $m_1$ 메시지를 만들어서 다음 에이전트에게 전달한다. 
(우) 정보의 주관성을 고려하는 메시지 전달 방식은 에이전트가 깊은 신경망으로부터 처리하는 정보의 주관성을 고려하여 메시지를 종합적으로 전달한다. 
</figcaption>
</figure>


그러나 처리과정을 모두 전달하는 것은 정보전달의 측면에서 효율적이지 못하다. 따라서, 가장 처음과 끝 정보만 전달하는 게이트 기반 정보전달 방식을 제안한다. 게이트 기반 정보 전달은 기존에 제안되었던 방식이나, 다중-에이전트 환경에서 제안하는 것은 본 논문이 처음으로 제안하는 내용이다. 

전달자 에이전트 (sender agent) 의  $t$ 시간에 관찰값을 $o_t^{(s)} \in \mathbf{R}^{obs}$,  신경망으로부터 만든 표현값을 $h_t^{(s)} \in \mathbf{R}^d$ 라고 하자. 전달자 에이전트는 두 가지 정보를 다 송출하고, 수신자 에이전트 (receiver agent) 는 두 정보에 대해서 적절하는 섞는 방식으로 정보를 처리한다. 이 때, 선형보간값 $\sigma^{(r)}$ 를 사용하여, 
선형보간된 정보 $g_t^{(r,s)}$ 를 생성한다. 에이전트는 두 가지 정보중에서 필요한 정보에 가중치를 주고 선택하게 된다. 이러한 방식은 게이팅 방식 (Gating Mechanism)으로 필요한 정보에 대한 회로를 열어서 정보를 전달한다. 

$$
g_t^{(r,s)} = \sigma_t^{(r)} h_t^{(s)} + (1 - \sigma_t^{(r)})  \hat{o}_t^{(s)} 
$$

이 때, $\hat{o}$ 은 관찰값의 차원을 선형변환하여, 신경망의 표현값과 동일한 사이즈로 만든 값이다. 

## 메시지 전달의 간단한 형태

다중 에이전트의 메시지 전달을 분선하기 앞서서, 다중 에이전트 상황에서 메시지 전달의 의미를 기반으로 간단한 실험을 먼저 진행한다. 
에이전트간의 메시지 전달은 전달한 메시지가 사용하는 쪽에서 강화학습 최적화 시그널로 학습되는 것이다. 따라서, 메시지 전달은 수신자의 메시지가 송신자의 강화학습으로 학습되는 구조이다. 
이는 단순히 하나의 에이전트에서 두 개의 신경망 모듈을 지니고 학습하는 것과 동일한 상황이다. 
따라서, 단일 에이전트에서 두 모듈을 사용하여 메시지 전달의 안정성에 대해서 먼저 분석한다. 
그림은 두 모듈에 대해서 가능한 세 가지 메시지 전달 방식을 보여준다. 추가적으로 메시지의 안정성을 확인하기 위해서, 메시지의 약간의 노이즈를 더하는 방식으로 메시지를 변형하였다. 


<figure style="text-align:center">
<img align src="/assets/side_papers/subjectivitiy_information/cartpole_messages.png" style="width:70%">
<figcaption>
메시지 전달의 세가지 방법
</figcaption> 
</figure>


* 방법 1 (Raw Observation Message)

제일 먼저 확인해야 하는 것은 에이전트가 관찰했던 값을 그대로 사용하는 방법이다. 
이는 신경망으로 메시지를 만드는 관점에서 봤을 때, 신경망이 항등함수인 경우와 동일하다. 
에이전트가 받는 메시지는 이전 에이전트의 관찰 상태이다. 

*  방법 2 ( Deep Neural Network Message)

두 번째 방법은 네트워크로부터 관찰값을 가공하여 전달해주는 것이다. 
이 방법은 기존 다중 에이전트 메시지 전달 방법과 동일하다. 


* *방법 3 (Linear Interpolation)

세 번째 방법은 관찰값과 딥러닝 메시지를 에이전트가 직접 고르는 방식이다. 에이전트는 적절한 비율로 관찰값과 심층표현을 선형보간하는 방식으로 학습한다. 
만일 전달된 메시지가 심층표현이 필요하지 않는다면, 에이전트는 단순히 다른 에이전트의 관찰값을 사용할 것이다. 



아래 그림은 CartPole-V0 환경에서 100K 샘플에 대해서 세가지 방식으로 학습한 결과이다. 결과는 2개의 랜덤시드를 평균하여 나타냈고, 95% 신뢰도 구간을 표현하였다. 게이트 기반의 에이전트는 노이즈가 없거나 적은 환경에서 단순 관찰값 기반 에이전트보다 높거나 안정적인 성과를 보였다. 그러나, 노이즈가 심한 환경에서는 단순 관찰값 기반 에이전트보다 낮은 성능을 보였다. RNN 심층표현 기반 에이전트는 학습의 안정성으로 인해서 성능이 좋지 못한 것을 확인할 수 있다. 

<figure style="text-align:center">
<img align src="/assets/side_papers/subjectivitiy_information/cartpole_return.png" style="width:100%">
<figcaption>
Figure. The training return in CartPole-v0 environment over 100K samples averaged by 2 seeds.
</figcaption> 
</figure>


<figure style="text-align:center">
<img align src="/assets/side_papers/subjectivitiy_information/cartpole_vf.png" style="width:100%">
<figcaption>
Figure.  The error of the value function in CartPole-v0 environment averaged over 2 seeds. 
</figcaption> 
</figure>

게이트 기반 에이전트를 더욱 분석하기 위해서, 선형보간에 사용된 값을 분석하였다. 아래 그림은 선형보간 초기값 1로부터 학습이 진행됨에 따라서 증가하는 경향성을 보여준다. 이 경향성은 1.2 수준에서 멈췄는데, 이는 심층망 기반 표현값을 0.2% 정도 섞는 것으로 고려할 수 있다. 


<figure style="text-align:center">
<img align src="/assets/side_papers/subjectivitiy_information/alpha.png" style="width:70%">
<figcaption>
Figure.  the interpolation value over training averaged over 2 seeds. 
</figcaption> 
</figure>





## 다중에이전트 학습

본 논문에서 실험은 에이전트 간의 커뮤니케이션이 중요하고, 전달하는 입력 정보가 그 자체로도 쓰임새가 있는 환경을 선택하였다. Mate 환경은 타겟들이 정보를 전달하고 여러 대의 카메라가 정보를 타겟을 감시하는 환경이다. 이 때 강화학습으로 학습한 에이전트는 카메라로 타겟들은 탐욕적인 방식으로 물품을 옮긴다고 가정하였다. 카메라들은 타겟들을 감시한 횟수에 따라서 보상을 받게 된다. 

<figure style="text-align:center">
<img align src="https://user-images.githubusercontent.com/16078332/130274196-9d18563d-6d42-493d-8dac-326b1924d2e3.gif" style="width:30%">
<figcaption>
© Image From   <a href="https://github.com/UnrealTracking/mate">MATE github</a>
</figcaption>
</figure>


멀티에이전트 모델은 기존 TarMAC 을 사용하였고, 메시지 전달 방식을 아래 그림과 같이 수정하였다. 전달자 에이전트가 신경망 기반 표현값과 관찰값을 모두 전달해주면, 수신자 에이전트 쪽에서 값을 게이트를 활용하여 필터링하는 방식이다. 이 때, 신경망 기반 표현값은 RNN을 통해서 만들어진다. 


<figure style="text-align:center">
<img align src="/assets/side_papers/subjectivitiy_information/simple_communication.png" style="width:100%">
<figcaption>
첫 번째 에이전트는 메시지를 만들고, 관찰값과 함께 두 번째 에이전트에 전달한다. 두 번째 에이전트는 메시지를 받으면 게이트 방식으로 처리하여 선형보간된 값을 사용한다. 
</figcaption> 
</figure>


아래 그림은 2vs2 와 4vs2 환경에서 4M 샘플에 대해서 학습한 결과를 보여준다. Vs 앞에 수는 카메라 개수를, 뒤에 수는 타겟 개수를 나타낸다. 
에이전트간 커뮤니케이션이 적은 2vs2 환경에서는 단순히 관찰값을 전달한 것이 성능이 높았다. 또한 심층망 표현값을 사용한 경우, 성능의 편차가 심한 것을 확인할 수 있다. 반면에 게이트 기반 메시지 전달은 편차가 적다. 4vs2 환경에서는 게이트 기반의 메시지 전달이 높은 성능을 보였는데, 이는 에이전트간 메시지 전달이 활발하고 전달하는 메시지가 객곽적인 정보와  주관적인 정보를 모두 포함해야 높은 성능을 가짐을 나타낸다. 

<figure style="text-align:center">
<img align src="/assets/side_papers/subjectivitiy_information/mate.png" style="width:100%">
<figcaption>
Figure. The training return in MATE environment over 4M samples averaged by 2 seeds. 
</figcaption> 
</figure>

## 결론 

본 연구에서는 다중에이전트에서 게이트 기반 메시지 전달을 제안한다. 제안한 게이트 기반 메시지 전달은 심층신경망의 불안정성을 기반으로 하며, 실험적으로 적절한 게이트 방식이 효율적임을 보였다. 
본 연구에서 발견한 점은 게이트 방식을 초반에 관찰값 기반으로 하면서 점진적으로 심층신경망 기반 메시지를 첨가하는 방식이 효율적이다는 점이다. 이러한 안정적인 메시지 생성은 불안정한 다중강화학습에서 
보다 안정적인 학습, 빠른 학습 결과를 이루는데 도움이 될 것으로 기대된다. 


