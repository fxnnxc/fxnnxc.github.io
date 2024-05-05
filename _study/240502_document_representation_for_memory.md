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
3. 문서 메모리는 어떤 레이어에 넣어야 좋은가? 

## 코드 먼저 

세 가지 연구에 대해서 내가 가지고 있는 한계는 아이디어 부족이 아니라 실험부족이다. 
제대로 실험하여 동작한다는 것을 보여야 한다. 

* github : llm (experiment 1) : [240503_docreps](https://github.com/fxnnxc/llm/tree/v24.05.04.DocRep) (document guidance loss)
* github : llm (experiment 2) : [ bias]() and memory selection

# 실험 결과


### Different Document Representations 

미분을 진행하였을 때, 문서 표현의 변화는 적다. 이는 Tanh와 ReLU 모두에 대해서 발생하는 현상이다. 논문에서 언급했던 smooth한 변화는 문서의 표현이 컴팩트하여 dimension이 document 개수와 비례하는 경우에 발생한다. 만일 문서 개수가 적고 dimension이 크다면, 문서 표현 변화는 거의 일어나지 않는다. 

문서 표현 변화가 적었던 또 다른 이유는 문서 표현의 dependency가 없었기 때문이다. 문서 벡터는 context vector와 역할이 비슷하다. gradient descent로 변화되는 표현을 살펴봤지만, 눈에 뜨게 음수와 양수가 바뀌지 않았다. 따라서 미분으로 적합한 표현을 찾는 것은 

한 가지 가설은 linear 연결하는 것이 큰 변화를 유도하기 힘들다는 것이다. 
non-linear로 문서표현으로부터 키를 선택한다면, forgetting loss에 대해서 문서 변화가 더 클 수 있을까? 
이런 부분들은 gradient 계산, 입력과 출력의 관계들을 조목조목 파해쳐야 알 수 있다. 아직 그 부분까지 이해는 부족하다. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%218862&authkey=%21AOct9LhqPJxUzcI&width=660" width="660" height="auto" />


### Inductive Bias and Memory Selection

