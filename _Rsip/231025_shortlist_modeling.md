---
layout: share-distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2023-10-20
featured: true
img: 
title: '[6] SIP with BCE for extended 1000 classes with Shortlist Modeling <strong> [KSC 2023] </strong>'
description: '100개의 클래스를 넘어서 1000개의 클래스에 대해서 학습을 유도한다.'
---

<div class="spanbox" markdown="1" style="width:40rem;background-color:#FFFDFD;">
👨🏻‍💻 **Code Release**:  Tag [v23.10.20.1](https://github.com/fxnnxc/source_identification_problem/tree/v23.10.20.1)
</div>


## 이전 연구 

이전 연구에서 100개의 클래스에 대해서 모델별 차이를 확인했다. 이번 연구에서는 원천소스 맵핑을 위해서 1000개 이상의 클래스에 대해서 학습을 진행한다. 
이전 연구 결과에서 살펴본바로는 100개를 학습하는데 있어서, 클래스에 대한 확률분포를 유도하는 `CE Loss` 보다 클래스마다 Logit 을 계산하는 `BCE Loss` 가 안정적인 것을 확인하였다. 더 많은 클래스에 대해서 `BCE Loss` 를 사용하는 경우, 학습의 효율성에 대해서 문제가 생길 수밖에 없기 때문에, 이번 연구부터는  extreme classification 의 영역이다. 해당 분야에서 많이 사용되는 `Aztec` 프레임워크는 1차적인 학습과 2차적인 Negative Label Sampling이 존재하고, 마지막으로 원 문제에 대한 예측문제를 푼다. 본 연구는 원천소스 1000개의 클래스에 대해서 단순 `BCE Loss`를 적용하여 푼 것과 `Aztec` 프레임워크를 적용하여 풀었을 때, 학습의 차이를 살펴본다. 이에 따라 레이블 수에 따른 예측 문제의 확장성에 대해서 결론을 도출한다. 

 

## Aztec 


1. Pseudo Label Feature Learning. 

* Sentence Similarity 기반 Pseudo Label : 추가적인 Similarity 값이 필요하다. 
* Vector Representation 기반 클러스터 형성 


2. Negative Label : 벡터 표현 자체가 유사한 애들에 대해서 구분한다. 그렇다면 결국 1번에서 만든 In-class 에 대해서 하는 것과 동일하지 않은가? 



## 실험 


