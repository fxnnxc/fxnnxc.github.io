---
layout: page
title: LRP 에 관련된 오해와 고민 
description: a project with a background image
img: 
importance: 2
category: work
---


## LRP 와 관련된 고민 

### 첫번째 고민 
첫번 째 걱정은 LRP 모델을 믿을 수 있는가에 대한 문제이다. 
내가 구현한 것과 다른 사람이 구현한 것은 LRP mechanism 상 차이가 없어야 한다. 
이에 관련된 룰을 다른 곳에서 구현한 것을 보고, LRP model 을 더욱 신뢰할 수 있어야 한다. 
현재는 construction이 꽤나 불안정한 상태이다. 고쳐야 한다. 

### 두번째 고민 
LRP 를 기반으로 학습과정에서 발견되는 특징을 설명하려고 하는데, 동적인 강화학습과 정적인 LRP에 대한 고민이 있다. 
강화학습은 보상을 최대한으로 얻기 위해서 움직인다. 
LRP가 크게 활성화되는 것과 이 정보는 어떠한 관계가 있는가? 
측, input attribution 이 큰 경우, 강화학습에 대해서 어떠한 사실을 말해줄 수 있는가?

PPO로 학습할 경우, 행동에 대해서 가장 좋은 행동을 선택하게 된다. 

* LRP가 활성화되면, 해당 정보를 더욱 이용하는가? 
* 그렇다면 학습 자체는 해당 정보를 더욱 이용하고 싶어한다는 걸 반증하는 건가? 
* Feature 들은 상대적으로 비교할 수 있다. Mountain Car에는 두 가지 observation space가 있고, LRP 정보는 둘 중 어떤 게 더 중요한 지 알려준다.
  
* 특정 상태에 대해서 LRP 정보가 학습되는 것은 모델이 해당 정보를 더욱 활용한다는 것을 나타낸다. 
* LRP의 변화를 관측하는 것의 정보의 활용에 대한 dynamics 를 분석하는 것이다. 
* 