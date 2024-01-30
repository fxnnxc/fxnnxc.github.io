---
layout: share-distill
title: '[MARL-Message-Adaptor] 1. Framework '
date: 2024-01-25
description: Project
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
img : ''
---


## 메시지에 의한 행동 차이 중요성 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21900&authkey=%21AN3mJJl1JF5cWZI&width=853&height=480" width="853" height="480" />

[gif rendering issue.. GIF 이미지 링크](https://1drv.ms/i/s!Asr4ZEBiKgSuhwVxE_iNwBLoepP8?e=wgyqkv)

---

## 1. 메시지 어뎁터 방식 

어뎁터는 기존 에이전트의 행동에 대한 정보를 바탕으로 이를 보정하는 메시지 기반 모듈을 추가학습하여 모델을 개선한다. 


## 2. 액션 메시지 정의

1. 기존 : 에이전트가 다른 에이전트에게 전달할 메시지를 만든다. 
2. 본 연구 : 에이전트가 본인의 행동과 관찰에 대해서 다른 메시지에게 전달한다. 

따라서 본 연구에서는 메시지 자체가 개별 에이전트가 결정된 행동 및 관찰 정보로 부터 유용한 정보로 정의한다. 
행동을 결정할 당시 전달할 수 있는 메시지는 세 가지가 존재한다. 각 에이전트는 아래 정보들을 생성하여 다른 에이전트에게 전달한다. 

* 처리된 관찰 값 : $h_i$
* 행동 및 확률 : $a_i, p_{a_{i}}$
* 다른 행동에 대한 확률 : $P_{a_{i}}$

$$
m_i = (h_i, a_i, p_{a_{i}}, P_{a_{i}})
$$


### 3. Processing Multiagent Messages 

임의의 개수의 에이전트가 주는 메시지를 처리하기 위해서는 데이터를 pooling하는 것보다 RNN 으로 처리하여 정보를 최대한 보존하는 게 유리하다. 따라서 GRU 방식으로 메시지를 처리하였다. 
(단, action_v1 만 구현된 상태고, message_v은 RNN이 아닌 Pooling 방식이다) 

### 4. 실험 결과 

#### 최종 보상 

학습은 각 에이전트가 관찰로부터 보상을 최대화 하는 Step1 이후, 통신메시지를 결합하는 Step2를 진행하였다. 
학습 시간은 2.5M이다. 아래 표는 학습 완료된 모델에 대해서 100 에피소드에 대한 보상의 평균과 분산을 나타낸다. 
spread 환경에서는 통신을 통해서 모두 성능이 개선되었다. 그러나, reference 환경에서는 통신을 하는 것이 오히려 불안정성을 높혔는데, 이는 reference 에서는 이미 Step1에서 필요한 정보가 모두 존재하며, Step2는 보상최대를 위해 필요한 정보가 추가되지 않기 때문이다. Simple Tag 에서는 기존 Step1보다 성능이 저하되는 경우도 존재하였기에, 메시지 종류에 따라서 다른 성능을 보이는 것으로 확인된다. 

| Environment |Model| Step1 Return| Step2 Return|
|---|---|---|---|
mpe.simple_tag_v3  |  message_v1 | $34.30 \pm(26.73)$ | $\textcolor{green}{36.20 \pm(29.56)}$ | 
mpe.simple_tag_v3  |  action_v1 | $33.60 \pm(28.76)$ | $\textcolor{red}{31.00 \pm(23.94)}$ | 
mpe.simple_reference_v3  |  message_v1 | $-40.36 \pm(14.91)$ | $\textcolor{red}{-46.29 \pm(19.25)}$ | 
mpe.simple_reference_v3  |  action_v1 | $-39.76 \pm(14.39)$ | $\textcolor{red}{-54.57 \pm(19.13)}$ | 
mpe.simple_spread_v3  |  message_v1 | $-65.79 \pm(35.68)$ | $\textcolor{green}{-54.89 \pm(18.29)}$ | 
mpe.simple_spread_v3  |  action_v1 | $-64.76 \pm(32.98)$ | $\textcolor{green}{-54.85 \pm(18.22)}$ | 

### 학습 다이나믹스

#### Step2: Adaptor 학습 비교 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21899&authkey=%21ACPyZ9dgwToCcLw&width=660" style='border:1px solid #000000' width="660" height="auto" />


<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21901&authkey=%21AAPm-_KxLiklRNQ&width=806&height=246" style='border:1px solid #000000'>

## 결론

메세지 adaptation은 일반적으로 더 높은 성능이 나오도록 학습된다. 
그러나 어떤 메시지를 주는 지에 따라서 학습 성능이 다르다. 따라서 각 환경에 적합한 메시지의 형태를 고려할 필요가 있다. 
논문의 주제는 message adaption framework의 제안. 메시지의 다양한 형태에 대한 제안이다. 

1. 심층 메시지는 항상 효율적인가? 아니다. 환경따라 다르고, 메시지 타입 따라 다르다 (정보가 많은 것은 그만큼 많은 처리를 필요로하고, MARL에서 더 오랜 시간 학습을 필요로 하고, 불안정성이 높다.) 
2. 심층 메시지의 효과를 나눠서 확인하는 방법은 무엇인가?, Message Adaptor framework
3. 심층 메시지의 효과를 개선하는 방법은 무엇인가? Action Information Message 

--- 



## 정리 안됨 

1. 메시지는 굉장히 노이즈한 경향성을 보인다. 
2. 메세지에 의존해서 전체 모델을 학습하는 것을 위험하다. (메시지가 noisy 한 경우는 어떻게 되는가? 잘 안되겠지. 불안정하겠지.)
3. 구조적으로 메시지 노이즈를 처리할 수 있는 모듈은 어떤 형태인가?

멀티에이전트에서 통신한다는 것은 정보교환이 발생하는 것을 나타낸다. 협업을 위해서 통신은 필수적이고, 
이에 대하 효과를 명시적으로 확인하기 위해서는 통신의 효용만큼 에이전트의 행동이 개선되어 성능 향상을 보여야 한다. 

멀티에이전트 환경에서는 관찰, 에이전트의 행동과 같은 기본 요소들이 통신과 더불어 반응하여 기존 에이전트의 행동을 제대로 구분되게 보여주지 못한다. 
각 에이전트의 컨트롤에 대한 부분과 통신에 대한 부분을 명시적으로 구분하는 것은 두 가지 장점이 있다. 

1. 기존 비통신 대비 성능 향상. 성능 향상에서 통신에 의한 행동 변화를 보여줌. 
2. 행동 자체의 변화에 대해서 해석할 수 있다. 



통신의 효과를 명시적으로 확인하고 

## 성능 비교 

행동을 관찰하는 것과 관찰 메시지를 전달받는 것은 성능 차이를 보이는가? 
행동을 관찰하는 것과 의도에 대한 메시지를 전달받는 것은 성능 차이를 보이는가? 

|환경 | 에이전트 관찰 | 에이전트 관찰 메시지 | 에이전트 관찰+행동 메시지 |
|  simple_spread_v3 | 000 | 000| 000|
| simple_reference_v3 | 000| 000| 000|
