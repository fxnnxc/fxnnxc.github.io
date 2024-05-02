---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-05-02
featured: true
title: '[Draft] Document Representation and Memory'
description: '두 가지 의문. 1) 문서에 대한 표현으로부터 메모리의 엔트리를 만드는 것은 실제로 효과가 있는가? 2) inductive bias는 실제로 효과가 있는가?'
---


## 서론 

메모리의 설계에 대해서 (나는 이제 메모리 설계라는 단어를 사용하기 시작했다.),
제대로 된 논의를 위해서는 두 가지 연구를 시작해야 한다. 

1. 문서 표현으로부터 메모리를 설계하는 것. 
2. 문서 메모리 학습과정 시각화 (만일 메모리 선택이 고정되지 않았다면,)
3. 문서 메모리 inductive bias for token-based memory selection. 


## 코드 먼저 

세 가지 연구에 대해서 내가 가지고 있는 한계는 아이디어 부족이 아니라 실험부족이다. 
제대로 실험하여 동작한다는 것을 보여야 한다. 

* github : llm (experiment 1) : 240503_docreps (document guidance loss)
* github : llm (experiment 2) : visualization of memory selection loss and logit lens 
* github : llm (experiment 3) : inductive bias and memory selection

# 실험 결과


### Different Document Representations 


### Visualization of Memory Selection and Logit Lens 


### Inductive Bias and Memory Selection

