---
layout: default
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-04-30
featured: true
title: '[Draft] Indirect Effect with Hook and Sally-Anne Test'
description: '[baseline] 실험 구현 및 평가'
---

* Code for ToM : [github](https://github.com/fxnnxc/llm/tree/v24.04.29_ToM)
* Code for ToM Hook

## 모델 평가 Reproducibility 

일단 데이터를 믿기 어렵다. 생성하는 방식 및 평가하는 방식에 따라서 결과값이 계속 달라지게 된다. 데이터에 문제가 있었다. 특정 레이블 (B) 값이 정답으로 사용되었는데, LLM이 단순히 B값을 예측하게 되는 경우 정답율이 100% 가까이 나왔다. 이를 개선하기 위해서 레이블을 섞었고, 정확도는 떨어지게 되었다. 데이터에 대해서 모델의 결과를 평가하는 경우 입력과 출력이 공정하게 형성되었는지 파악하는 것이 중요하다. 

입출력 값이 편향적으로 생성되는 경우, 예측 결과를 신빙할 수 없다. 예를 들어서, 90:10으로 데이터의 분포가 이루어지는 경우, 90으로 찍어서 정확도 90%가 나오는지, 아니면 balance를 비슷하게 유지한 상태에서 90%가 나오는지 알기 어렵다. 따라서 모델의 정확도를 측정하기 위해서 정답은 될 수 있는한 찍기 어렵게 만들어야 한다. 그러나 QA 형태로 closed answer를 하는 경우 제대로 된 예측이 어렵다. 이를 해결하기 위해서 ToMChallenge 논문<d-cite key="ma2023tomchallenges"></d-cite>에서도 autograder라는 방식으로 도입했다. 이 방식은 GPT4를 사용하기에 실요성이 낮지만, ToM을 평가하기 위해서 필요한 방법인 것 같다. ToM은 일반적으로 인간의 높은 인지 수준을 요구한다. 따라서 레이블을 사람이 직관적으로 한 눈에 예측하기 어렵다. 


## 믿을만한 ToM 결과 

일반 적어도 모델이 어느 부분에 있어서 어느정도 ToM을 보여야 한다. 
그런데 context도 길고 모델이 제대로 이해하고 대답하는지 잘 모르겠다. 

일단 기대한대로 reality에 대한 예측은 높게 나와야 한다. 
나머지 부분에 대해서는 second-order mind로 갈수록 예측 정확도가 떨어져야 한다. 
실험 결과는 예상한대로 나왔다. 

한 가지 방법은 정답에 대해서 다양한 형태로 예측을 하여 gold answers 를 생성하고 가장 높은 정확도를 사용하는 것이다. 

* 예시: A,B 모두 타월이 바구니에 있는 것을 확인하였고, B가 없을 때 A는 타월을 쓰레기통에 넣었다. 

* 1stA: A는 현재 타월이 어디 있다고 생각하는가? (쓰레기통)
* 1stB: B는 현재 타월이 어디 있다고 생각하는가? (바구니)
* 2ndA: A가 생각하는 B의 타월 추정 위치는? (바구니)
* 2ndB: B가 생각하는 A의 타월 추정 위치는? (바구니) 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%218756&authkey=%21AOcn77RvPEvB8wo&width=785&height=661" width="785" height="661" />

Reality와 First A에 대해서는 정확도가 상대적으로 높은 것을 볼 수 있다. 반대로 1stB와 2nd에 대해서는 정확도가 대단히 낮다. 

<blockquote>
사실 이 부분을 제대로 측정하기 위해서는 정답의 반대 레이블에 대한 믿음이 높은지 확인해야 한다. 해당 점수에서는 틀린 것에 대해서 hallucination을 포함하고 있기 때문이다. 
즉, 0% 정확도가 나오는 부분이 ToM을 실패해서 나온 것인지 모델의 예측이 포맷이 안 맞아서 그런 것인지 확인해야 한다. 
</blockquote>

