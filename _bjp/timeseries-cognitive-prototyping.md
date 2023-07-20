---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
    - name: Jihyeon Seong
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-07-18
featured: true

title: 'Time series Cognitive Prototyping'
description: 'How to prototype time series data?'
img:
importance: 2
---


## Time-series Cognitive Prototyping 


사람은 그래프데이터를 봤을 때, 가장 먼저 데이터의 형태를 파악한다. 값이 올라가는지, 내려가는지, 꺾이는 부분이 있는지. 이로부터 데이터를 해석하는 사람들은 패턴을 찾으려고 노력하고, 패턴을 찾는 경우, 그게 얼마나 중요한지 측정한다. 아래 그림을 보고 패턴을 추출하기 위해서 어떠한 추론 과정을 거치는지 생각해보자. 
<img src="https://www.scylladb.com/wp-content/uploads/time-series-data-diagram.png" style="width:800px">


사람이 봤을 때 **데이터를 해석하는 과정은** 과정은 다음과 같다. 
> 
1. 그래프 관찰 
2. 그래프 주요 패턴들 관찰 **(상대적 위치들)**
3. 그래프 Segment **(Variational Size)**
4. 패턴들의 중요성 측정
5. 다음 패턴들 유추 

### Timeseries Models

그러나, 이와 다르게 딥러닝 모델들은 관찰로부터 패턴을 명시적으로 찾고 값을 예측하는지 확실하지 않다. 딥러닝의 주요 처리 과정은 다음과 같이 생각할 수 있다. 
> 
1. 그래프 관찰 
2. 그래프 주요 패턴 관찰 **(절대적 위치들)**
3. 그래프 Segment (불확실, 모델 내부에서는 할 수도)
4. 패턴들의 중요성 측정
5. 다음 패턴 유추

따라서, 딥러닝 모델은 좀더 **값에 의미를 두는 경향이** 있으며, 이는 모델과 차이가 발생한다. 그러나 패턴을 찾는 사람의 인지적인 과정은 현재 데이터를 분석하는데 필요할 뿐만 아니라, 앞으로 발생할 사건을 예측하는데 중요하다.  이는 Hawk Process (From 지현) 와 유사하게, 패턴을 찾는 문제는 이벤트를 찾는 문제이며, 시계열 문제는 이벤트에 대한 절대적인 값을 부여하는 것이다. 
따라서, 인지적인 사고과정을 포함한 모델링은 다음과 같다. 

* `Forecasting` : 시계열 데이터 + (Option:**인지적 패턴들**) → 다음 인지적 패턴  유추 →  인지적 패턴 기반 Forecasting 
* `Classification`:  시계열 데이터 + (Option:**인지적 패턴들**) → 레이블

따라서, 궁극적으로는 인지적패턴들을 레이블하고, 이를 기반으로 사후적인 시계열 문제를 푸는 방식으로 접근해야 한다. 
