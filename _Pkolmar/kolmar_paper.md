---
layout: share-distill
title: '논문 연구: 확장가능한 인공지능 기반 처방 물성 예측'
date: 2024-01-10
giscus_comments : true
description: "처방 정보를 기반으로 다양한 물성 예측하는 방법론"
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: kolmar.bib
# img: https://drive.google.com/uc?export=view&id=1K6nvmbzhEsk2aSZyRh98l7ZaeilIPMAr
img: https://onedrive.live.com/embed?resid=AE042A624064F8CA%21833&authkey=%21AA9mhP3YbvPbpfI&width=2798&height=1488
---


## Scalable Materiality Prediction of Cosmetic Recipe with Artificial Intelligence

### 1. 소개

#### 1.1 처방 물성 예측의 필요성
1. 화장품을 처방을 만들 때, 처방과 물성을 파악하는 것은 중요한 연구주제다. 
2. 처방에는 수많은 원료들이 들어가고 처방으로부터 물성을  추정하는 것은 쉽지 않다. 
3. 처방의 물성을 실제 검증할 수 있지만 측정하는 과정에서 시간과 노력이 든다. 
4. 따라서 처방으로부터 물성을 사전에 예측하는 모델은 비용을 절감하는데 도움이 되며, 모델의 예측 과정에 대한 분석은 처방에 대한 새로운 인사이트를 사용자에게 제공할 수 있다. 

#### 1.2 문제 정의 
1. 최근 연구에서는 AI를 기반으로 물질의 성질을 예측하는 연구가 진행되었다. 
2. 본 연구는 딥러닝을 활용하여 화장품 처방과 물성의 관계를 학습하는 방법을 연구한다. 
3. 처방과 물성의 관계를 연구하는데 있어서 걸림돌은 다음과 같다. 
    1. 원료의 종류는 회사마다 다르며, 원료의 수와 특징은 굉장히 다양하다. 예를 들어서, ABC111 이라는 원료가 혼합물(A+B+C)로 이루어져있을 수 있으며, 원료의 고유한 특징이 복합적일 가능성이 높다. 또한  ABC111은 회사 마다 다르다. 
    2. 데이터로 준비할 수 있는 처방의 수는 적다. 실제 연구자가 처방으로 작성하여 물성을 테스트한 결과는 대부분 많이 모으기 쉽지 않다. 
4. 본 연구는 이러한 두 가지 한계를 인지하고, 이를 해결하기 위한 **인공지능의 구조적 방법론에** 대해서 연구한다. 

#### 1.3 본 연구의 기여 
본 연구는 처방과 물성의 관계를 학습하는 방식을 표현벡터의 관계로부터 학습하는 방식을 제안한다. 
원료와 물성에 대한 표현 벡터를 기반으로 처방 정보와 여러 물성에 대한 일반화되고 scalable 한 형태의 구조를 제안하며, 
콜마의 10K개의 처방에 대한 테스트 성능은 해당 모델 구조가 임의의 원료 및 물성에 대해서 높은 예측 성능을 보임을 보여준다. 
또한 제안하는 모델 구조는 처방에 대해서 원하는 물성에 대한 가중치를 바탕으로 하여, 물성에 대해서 처방의 원료 가중치를 판단할 수 있어, 
인공지능 모델로부터 인사이트를 얻을 수 있는 구조이다. 

### 2. 방법론 

1. 원료의 의미를 사전에 정의하는 대신, 각 원료가 임의의 표현이라는 가정. 
2. 물성이 예측 레이블이 아닌, 임의의 표현이라는 가정
3. 임의의 원료와 여러 물성을 동시에 처리하여 예측할 수 있는 구조적 방법론 

<center>
<figure style="width:70%">
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21833&authkey=%21AA9mhP3YbvPbpfI&width=2798&height=1488" style="width:100%">
<figcaption>
Figure 1. 심층신경망은 처방과 예측하는 물성에 대한 표현 벡터를 바탕으로 예측값을 내보낸다. 처방의 원료 개수는 제한이 없으며, 물질의 양은 다 더해서 1을 만족한다.  
</figcaption>
</figure>
</center>


###  3. 실험 

제안하는 구조는 
1. 단순한 모델부터, 복잡한 모델까지 모델의 복잡도는 물성 예측의 성능을 높이는가? (+머신러닝, 다른 approach와 제안하는 구조 성능 비교)
2. 원료의 표현 벡터가 길어짐에 따라서 예측 성능을 향상되는가? 
3. 원료에 대한 추가적인 정보는 도움이 되는가?  (배합목적, INCI 추가)
4. 적은 수의 처방을 가지는 신규 물성에 대한 적응은 뛰어난가? 
5. 모델과 원료 표현들은 해석가능하며, 인사이트를 얻을 수 있는가? 


## 결론

화장품의 처방과 이에 사용되는 원료는 복잡한 의미를 담고 있다. 사람이 원료의 특징을 정하는 것은 한계를 가지고 있으며, 
다른 원료들과 상호작용에 대해서 사람이 특징을 나열하거나 관계를 파악하는데 한계가 있다. 
본 연구에서 제안하는 원료표현 및 물성표현 방식은 딥러닝의 표현 벡터와 학습의 효과를 바탕으로 처방에 사용된 원료의 표현을 자동으로 찾기 위한
하나의 접근 방법을 제시한다. 실험으로 보인 딥러닝 모델의 복잡성과 성능의 상관관계는 처방과 물성의 관계가 복잡할 수 있으며, 
구조적으로 적합한 딥러닝적 모델이라면 이를 더 잘 학습할 수 있음을 보인다. 
처방을 통해서 얻어야 하는 정보는 무수히 많다. 필요한 정보를 모두 심층표현으로 나타낸 본 연구의 방법은 
추가되는 원료, 추가되는 물성에 유연하게 대처할 수 있음을 본 연구는 보였다. 

---

## Survey 

### Accurate prediction of dynamic viscosity of polyalpha-olefin boron nitride nanofluids using machine learning <d-cite key="abushanab2023accurate"></d-cite> 
 

