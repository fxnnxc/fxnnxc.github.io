---
layout: share-distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-07-25
featured: true
# toc:
#   - name: Problems
title: 'Modeling Sharing Contents in  MARL'
description: 'How can we improve the quality of messages in deep learning?'
img:
---


# Introduction 

Environment is a complex space where several agents and objects make interactions. 
In the environment, rational agents act to maximize utilities and the action is learned by reinforcement learning algorithm (RL) with a deep neural network. 
As the most of environments are non-stationary, a single agent can not achieve the maximum utility only with its observation and inferences with a network. 
However, if multiple agents can communicate with each other, a single agent can get richer information about the states of the environment by simply obtaining messages from other agents. 
However, the inferred information from other agents are made by a neural network which is sometimes vulnerable and includes the personal perspective of the agent processed the information. 
For example, even though there is a single observation, two agents can encode very different messages as parameters and modules could be different. 


Therefore, it is important to consider the subjectivity of messages in multi-agent reinforcement learning (MARL). 
Based on the assumption that the encoded information is a rather subjective information of the agent encode it, 
we tackle the problem of handling objective and subjective information in MARL. 
One limitation is that the processed information by a neural network is hardly interpreted. As the starting work of this subjectivity work, 
we tackle the most minor problem whether the subjective information is better than the objective information. 
In detail, we compare the usefulness of messages from an agent to another agent in two kinds: raw observation and the output of a neural network. 



# Preliminaries 

Before proceeding, we review the previous methods in MARL. 

> <strong style="font-style:normal">Learning Shared Q-values </strong> <br>
 **QMIX**<d-cite key="rashid2018qmix"/> learns a mixing network of Q-values which outputs the mixed Q-value which is monotonically  increasing for agent Q-values. **MADDPG**<d-cite key="lowe2017multi"/> utilizes the Q-functions conditional on broad casted information from agents and execute  agents in decentralized manner with the learned policy conditioned only on the observation of each agent.  
**MAPPO** (Multi-agent PPO)<d-cite key="yu2022surprising"/> utilizes the PPO with centralized value function inputs, while the value function of **IPPO** (Independent PPO)<d-cite key="de2020independent"/> takes an independent input.


> <strong style="font-style:normal">Communication Skills </strong> <br>
> **TarMac**<d-cite key="das2019tarmac"/> utilizes the attention mechanism between agents to spread the values between communicated agents. The recurrent hidden states outputs two components query and  [key, value] vectors.  **I2C**<d-cite key="ding2020learning"/> uses the prior network to determine whom to request a message. The difference with TarMac is the separation of communication steps. We believe I2C is discrete as due to the determination of agents to communicate. Note that TarMAC is based on Q-K communication. 

> <strong style="font-style:normal">Modeling What to Share </strong> <br>
**LToS**<d-cite key="yi2022learning"/> is a hierarchical modeling of agents (bi-level optimization) where the high-level policy distributes the reward signals to neighbors and the low-level policy control agent each. The shared information is the reward of each agent and the information can benefit the cooperative MARL. 

Note that our problem is in the field of **Communication Skills** and **Modeling What to Share**, and is orthogonal to the previous methods as we design the package style rather than communication styles. 



# Proposed Method 


Several MARL methods consider *how* to communicate between agents and make message with a deep neural network (DNN). 
As the output of the DNN automatically formed with the optimization algorithms, it is natural to think that the output of the agent is a compact information necessary to communicate between agents well. However, as the deep neural network is hardly interpreted, the passed message could be ambiguous and sometimes include errors. On the other hand, the observation of the agent is most unprocessed information which does not include the knowledge of the agent. Within the progress of the interpretation of DNN, we can find that the neurons in DNN are firing when they capture features in an observation. 

We believe that the simplicity of the observation is necessary for the message, but the message may include some errors induced from the agent.  Therefore, the receiver of the message may distinguish two information to decide whether to accept the processed information or decline it. 



<figure style="text-align:center">
<img align src="/assets/bjp/marl-what-to-share/subjectivity.png" style="width:100%">
<figcaption>
Comparison between the previous message passing and the subjectivity-based message passing. 
The previous method generate messages with a deep neural network and passed the information to the next agent. On the other hand, the subjectivity-based message pass a message which is gathered representations leveled by subjectivity. 
</figcaption>
</figure>



<figure style="text-align:center">
<img align src="/assets/bjp/marl-what-to-share/simple_communication.png" style="width:100%">
<figcaption>
The two levels communication packages. The raw observation and the processed information of the agent are passed to the second agent. The second agent process the information with gating mechanism to determine the necessary information. 
</figcaption> 
</figure>


# Why Subjectivity 

The general belief  of a DNN is that the hidden representation is a compact representation of the input. Therefore, it may not be necessary to gather information based on the subjectivity. 
However, the recent progress in the interpretability field have found that the internal neurons are activated by specific patterns and they form circuits for complex features <d-cite key="olah2020zoom"/><d-cite key="cammarata2021curve"/>. 
Therefore, it is natural to think that  two models can capture the very different concepts if their circuits are differently formed. In addition, the last output of a model is a complex feature of the input and could not encode enough information of the input. Therefore, the hidden representation have neither full input information nor common information between other models. 



# Learning to Choose

Let $o_t^{(i)} \in \mathbf{R}^{obs}$ be the observation of agent (i) and $h_t^{(j)} \in \mathbf{R}^d$ be the hidden message made by agent (j). 
The agent (j) will pass both information $(o_t^{(j)},h_t^{(j)})$ to the agent (i) who will choose which information to take. 

The encoder of agent (i) takes the input of the form:
$$
[o_t^{(i)}, (o_t^{(j)}, h_t^{(j)}), \cdots] 
$$

There are two operations: 
which agent to take and which type of information to take. 
First, encode its own information into $v_t^{(i)} = E_{obs}^{(i)}(o_t^{(i)})$. Then, compute the which information to take by sigmoid function $\sigma$. $r_t^{(j)} = \sigma(v_t^{(i)} \cdot h_t^{(j)})$ be the ratio of hidden representation.  $1 - r_t^{(j)}$ is the ratio of raw observation.  

$$
w_t^{(i,j)} = r_t^{(j)} h_t^{(j)} + (1 - r_t^{(j)})  o_t^{(j)} 
$$












# Benchmarking the Environments




# Environments 

MATE<d-cite key="pan2022mate"/> is a zero-sum cooperative MARL where cameras and targets are intra-teams communicate in each. 


<figure style="text-align:center">
<img align src="https://user-images.githubusercontent.com/16078332/130274196-9d18563d-6d42-493d-8dac-326b1924d2e3.gif" style="width:30%">
<figcaption>
© Image From   <a href="https://github.com/UnrealTracking/mate">MATE github</a>
</figcaption>
</figure>


## Reproducing Results of TarMAC


We initially run the experiments with 2 seeds. 

| num cameras | num targets | num obstacles | time steps  |  Done | 
| 2 | 2  | 0  | 1M | |
| 4 | 2  | 0  | 1M | |
| 4 | 4  | 0  | 1M | |


## Qualitative Verification of the Model 

How the gating mechanism is learned?


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
에이전트들을 학습하는 방식은 에이전트들의 정보교환의 정도에 따라서 중앙집중형과 분산형으로 구분될 수 있는데, 가장 많이 사용되는 방식은 학습 때는 중앙집중형으로 학습하고 테스트 때는 분산형으로 진행하는 (Centralized Training and Decentralized Execution) 방식이다. 이 방식의 대표적인 알고리즘들은 Q-MIX 와 MADDPG가 있다.  이들은 에이전트들간의 커뮤니케이션이 존재하진 않으므로 행동을 결정할 때는 에이전트가 본인의 관찰값만 사용하지만, 보상에 대한 정확한 예측을 위해서 종합된 정보를 사용한다는 특징이 있다. 이와는 반대로 행동을 결정할 때 에이전트들간의 커뮤니케이션을 하는 방법은 대표적으로 두 가지가 있다. 
TarMAC 은 에이전트들이 메시지를 보내고 받은 메시지를 종합적으로 고려하여 행동하는 방식이다. 그러나,TarMAC은 메시지가 항상 전달된다는 단점이 있는데, 이를 개선하여 요청-발신 구조를 제안한 I2C가 있다. 
두 커뮤니케이션 기반 에이전트들의 학습은 공통적으로 에이전트의 메시지가 신경망으로부터 생성되어 다른 에이전트로 전달되는 특징이 존재한다. 학습 과정에서 신경망은 지속적으로 변하며 전달하는 메시지 또한 동일한 관찰에 대해서 계속 달라질 수 있다. 이러한 전달하는 값에 대한 불안정성은 강화학습을 더욱 불안정하게 만들 수 있으며, 학습을 위해서 더 많은 시간이 필요하다. 이를 개선하기 위해서는 신경망으로부터 처리된 정보뿐만 아니라, 보다 에러가 적은 정보인 신경망의 입력 또한 에이전트에게 메시지로 전달해야 한다. 

# 제안하는 알고리즘 

기존 다중에이전트 알고리즘들은 에이전트간의 통신하는 방식에 대해서 논하면서, 전달 하는 메시지는 신경망으로부터 생성된 출력이었다. 생성된 출력은 입력에 대한 정보를 담을 수 있지만, 정보처리 관점에서 신경망이 깊어질수록 입력의 정보는 희석된다. 본 논문에서 제안하는 방법론은 입력 또한 중요한 정보이며, 때로는 신경망으로부터 생성된 정보보다 더욱 안정적으로 다중에이전트학습에 도움이 될 수 있다는 것이다. 그림1은 신경망이 깊어질수록 주관적인 정보가 점차 증가한다는 것을 보이며, 제안하는 방법은 단순히 신경망의 출력뿐만아니라 처리과정을 줘야 한다는 것이다. 

그러나 처리과정을 모두 전달하는 것은 정보전달의 측면에서 효율적이지 못하다. 따라서, 가장 처음과 끝 정보만 전달하는 그림2와 같은 정보전달 방식을 제안한다. 

## 구현 

에이전트는 두 가지 정보중에서 필요한 정보에 가중치를 주고 선택하게 된다. 이러한 방식은 게이팅 방식 (Gating Mechanism)으로 필요한 정보에 대한 회로를 열어서 정보를 전달한다. 


### 방법 1 (Raw Observation Message)

제일 먼저 확인해야 하는 것은 에이전트가 관찰했던 값을 그대로 사용하는 방법이다. 
이는 신경망으로 메시지를 만드는 관점에서 봤을 때, 신경망이 항등함수인 경우와 동일하다. 
에이전트가 받는 메시지는 이전 에이전트의 관찰 상태이다. 

### 방법 2 ( Deep Neural Network Message)

두 번째 방법은 네트워크로부터 관찰값을 가공하여 전달해주는 것이다. 
이 방법은 기존 메시지 전달 방법과 동일하다. 


### 방법 3 (Linear Interpolation)

세 번째 방법은 관찰값과 딥러닝 메시지를 에이전트가 직접 고르는 방식이다. 에이전트는 적절한 비율 $r$ 로 관찰값과 심층표현을 선형보간하는 방식으로 학습한다. 
만일 전달된 메시지가 심층표현이 필요하지 않는다면, 에이전트는 단순히 다른 에이전트의 관찰값을 사용할 것이다. 

$$
M_{interpol} (o, h)  = r_t^{(j)} h_t^{(j)} + (1 - r_t^{(j)})  o_t^{(j)} 
$$


# 실험 

본 논문에서 실험은 에이전트 간의 커뮤니케이션이 중요하고, 전달하는 입력 정보가 그 자체로도 쓰임새가 있는 환경을 선택하였다. Mate 환경은 타겟들이 정보를 전달하고 여러 대의 카메라가 정보를 타겟을 감시하는 환경이다. 이 때 강화학습으로 학습한 에이전트는 카메라로 타겟들은 탐욕적인 방식으로 물품을 옮긴다고 가정하였다. 따라서, 카메라들이 

기존 TarMAC 은 신경망으로 메시지를 생성하는 방식인데, 이에 대해서 개선된 두 가지 모델링을 검증한다. 
총 세가지 모델을 학습하였다. 


사용하는 환경은 총 세가지로 $2vs2$, $4vs2$ 에서 학습하였다. 



## 메시지에 따른 차이 




## 분석 