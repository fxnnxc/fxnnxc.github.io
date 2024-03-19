---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-03-17
featured: true
title: 'Side Information for Transformer'
description: '트랜스포머에 추가적인 입력을 같이 넣어서 생성물을 컨트롤 하는 연구'
---

[[🧑🏻‍💻 code: document_side_llm 24.03.18](https://github.com/fxnnxc/document_side_llm/tree/v24.03.18.1)]

### Side Information 과 Transformer 

추가 정보를 활용하는 방법을 설계한다. 모델은 정보를 저장하고 활용하며 구조는 정보를 처리하는 방식을 나타낸다. 
트랜스포머 모델의 연산이 지속적인 물음에 대해서 대답하는 기계라고 할 때, 연산처리를 위해서 필요한 구성 요소는 다음과 같다. 

1. <span style="color:#005500"> 질문 </span>
2. <span style="color:#005500">저장된 정보 </span>
3. <span style="color:#005500"> 정보 선택</span>

Transformer의 구조는 정보를 저장하는 MLP와 질문을 만들과 질문과 관련된 정보를 가져오는 attention으로 구성된다. 
질문에 대해서 정보를 활용하여 다음 단어를 예측하므로, 추가적인 정보를 활용하는 방법은 그 과정에 녹이는 것이다. 

### 추가정보의 활용

질문에 대답하는 기계에게 추가 정보는 세 가지 관점에서 모두 사용될 수 있다. 

1. 질문: 추가 정보는 적합한 질문을 개선하기 위해서 사용될 수 있다.  **q -> q’** 
2. 저장된 정보 : 추가 정보는 저장되어 있으며, 질문은 추가정보를 선택할 수 있다.  **(q,  [k, k’] [v, v’])**
3. 질문과 관련된 정보: 추가 정보는 기존 정보에서 정보를 선택하는 방법을 바꾼다. **A -> A + A’**


### 추가 정보의 변형 

추가적인 정보는 모델에 영향을 주는 부분과 별개로 모델을 지나면서 변형될 수 있다. 
뒤로 갈수록 연산의 복잡도는 올라간다.
문서의 정보를 활용한다는 입장에서 어떤 문제점이 있는지 질문 자체가 context에 맞게 바뀌는 게 좋을 거 같다. 
3번의 Key-Value-based Transformation이 좋을 거 같고, 근처 단어에 대한 local attention-bias가 유용할 것으로 추정된다. 
왜냐하면 문서 기반 생성은 bigram과 유사할 가능성이 높기 때문이다. 즉, 앞 부분의 context가 대부분 짧을 것으로 추정된다. 

1. **No Transformation**: 변형되지 않음
2. **MLP transformation**: 레이어를 지나면서 MLP로 인해서 바뀜. 각 레이어에 대해서 적합한 표현을 형성  
3. **Key-Value Transformation**: 기존 Transformer QKV연산을 동일하게 계산한다. 주어진 hidden state에 대한 변형을 요구한다. 

### 추가 정보의 의미 

위 두 가지에 대한 원칙을 세우면 추가 정보를 활용해서 가정에 대한 모델링이 가능하다. 

1. 질문은 어떻게 바뀌어야 하는가
2. 저장된 정보는 어떻게 바뀌어야 하는가
3. 정보의 선택은 어떻게 바껴야 하는가? 추가정보가 주어졌을 때 

### 모델에 편향성을 더하기

추가 정보를 활용하는 이유는 생성물을 컨트롤하기 위함이다. 예를 들어서 모델을 sentiment에 대한 conditional을 추가하여 학습했다고 하자. 
만일 모델이 긍정적인 추가정보를 주는 경우, 생성물은 긍정적인 발화로 구성될 것이다. 
반대로 모델에게 부정적인 추가정보를 주는 경우, 생성물의 감성 스코어는 전반적으로 낮은 스코어를 준다. 
그리고, 위에서 언급한 모델링은 Transformer 구조에 대해서 혹은 질문-지식-지식선택에 대한 편향을 주는 방식을 가정하는 것이다. 	


<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211285&authkey=%21AEIG1ULJ7wif3SE&width=1024" width="1024" height="auto" />

### 초기 실험 결과 및 피드백 

LLM을 처음부터 학습하는 것은 바람직하지 않다. 
모델에 대한 성능을 믿기 위해서는 모델 내부를 제대로 작성해야 한다. 
위에서 **추가적인 정보 변형**과 **추가적인 정보 활용**을 위한 모듈 구조에 대한 확신이 있어야 연구를 제대로 진행할 수 있다. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211287&authkey=%21AGPJKAcxvb6g5-I&width=566&height=471" width="566" height="471" />

**추가적으로 할 일**

* Sentiment 데이터에 대한 side information 같이 주는 방향 
* Pretrained Model의 Weight를 쓰는 방향
* Computational Logic에 대한 제대로된 설계 

--- 
