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
* github : llm (experiment 2) : layer-verification 

# 실험 결과

### Different Document Representations 

미분을 진행하였을 때, 문서 표현의 변화는 적다. 이는 Tanh와 ReLU 모두에 대해서 발생하는 현상이다. 논문에서 언급했던 smooth한 변화는 문서의 표현이 컴팩트하여 dimension이 document 개수와 비례하는 경우에 발생한다. 만일 문서 개수가 적고 dimension이 크다면, 문서 표현 변화는 거의 일어나지 않는다. 

문서 표현 변화가 적었던 또 다른 이유는 문서 표현의 dependency가 없었기 때문이다. 문서 벡터는 context vector와 역할이 비슷하다. gradient descent로 변화되는 표현을 살펴봤지만, 눈에 뜨게 음수와 양수가 바뀌지 않았다. 따라서 미분으로 적합한 표현을 찾는 것은 

한 가지 가설은 linear 연결하는 것이 큰 변화를 유도하기 힘들다는 것이다. 
non-linear로 문서표현으로부터 키를 선택한다면, forgetting loss에 대해서 문서 변화가 더 클 수 있을까? 
이런 부분들은 gradient 계산, 입력과 출력의 관계들을 조목조목 파해쳐야 알 수 있다. 아직 그 부분까지 이해는 부족하다. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%218862&authkey=%21AOct9LhqPJxUzcI&width=660" width="660" height="auto" />

### ReLU 및 Tanh 해석에 대해서 

ReLU는 음수를 0으로 만든다. 그리고, activation 값이 0인 경우, 이후 곱해지는 weight에 대해서 영향을 없앤다. 따라서, ReLU로 0을 만든다면, 특정 메모리의 값을 접근하지 않아서 값을 내보내지 않는다. 만일 ReLU로 positive한 memory selection이 가능하다면, zero는 메모리를 선택하지 않음으로써 기존 기여를 없앤다. 

그러나, positive 하게 값을 내보내도, weight 이 음수라면, 출력값은 음수가 된다. 따라서 곱해지는 weight이 양수 혹은 음수인지에 따라서 해석이 다른다. 

* 이후 곱해지는 weight이 양수이다. : ReLU의 positive output은 다음 뉴런을 활성화 하도록 만들고, ReLU=0은 뉴런을 키지 않는다. 
* 이후 곱해지는 weight이 음수이다. : ReLU의 positive output은 다음 뉴런을 inhibit 하도록 만들고, ReLU=0은 뉴런에 영향을 주지 않는다. 

따라서, ReLU=0이라고 해서 다음 특성을 끈다는 표현은 적절하지 않다. 올바른 해석은 다음과 같다. 

<blockquote>
ReLU값이 양수이면, 다음 뉴런의 역할을 강화한다. <br>
ReLU값이 0이면, 다음 뉴런에 영향을 주지 않는다. 
</blockquote>

메모리는 vocab 혹은 다음 표현에 대해서 값을 키우거나 줄이는 역할을 한다. ReLU가 켜진다면, 해당 메모리의 영향력을 키우는 것이다. 
이 때, forgetting과 memorizing context가 모두 존재한다. 따라서 메모리는 두 종류가 존재한다. 
1. 다음 특성의 표현을 높이는 메모리 (weight>0)
2. 다음 특성의 표현을 낮추는 메모리 (weight<0)


### 문서가 선택한 메모리 Pool은? 

문서 메모리의 역할은 문서에 해당하는 특징 혹은 단어의 확률을 높이는 것이다. 

만일 두 개의 메모리가 모두 $1$로 켜졌다고 하자. 확률을 높이는 방법은 두 가지이다.

1. 문서 특징의 확률을 높인다.
2. 문서 특징이 아닌 다른 특징들의 확률을 높인다. 

결과적으로 두 가지 메모리 모두 연산 후에 선택되는 단어들은 문서에 사용되는, 혹은 필요한 단어들이다. 

### 다시 Tanh해석으로 돌아와서 

Activation entry와 연결된 다음 뉴런은 downstream의 activation으로부터 호 (excitement) 혹은 불호 (inhibition)을 결절하였다. 
Weight이 음수라면 싫어하는 것이고, weight이 양수라면 좋아하는 것이다. 그러므로, activation 값의 의미는 다음 뉴런과 연결을 통해서만 해석될 수 있다. 
그러나 한 가지 확실한 것은 activation 값이 양수이고 크다면, 기존 뉴런의 stance를 그대로 가져가는 것이고, 0이라면 뉴런은 downstream branch로부터 영향을 받지 않도록 설계되었다. Tanh는 ReLU값에 추가로 음수인 값이 들어 있다. 

<blockquote>
음수는 다음 뉴런이 downstream에 대해서 지니는 해석 방식을 뒤집는다. 
</blockquote>

다음과 같은 상황을 가정하자. 

* 입력 A로부터 메모리 선택은 1이었다.
* 입력 B로부터 메모리 선택은 -1이었다. 

#### Additional Assumption 1 

해당 메모리는 단어 "hate"의 확률을 높이는 메모리였다. 


#### Additional Assumption 2

이제 다음과 같은 상황을 가정하자. 
해당 메모리가 단어 "hate"의 확률을 낮추는 메모리였다. 


---

## Which Layer is Helpful?

We conduct additional experiments to verify the relationship  between document-wise memories and the depth of LLMs. 
Settings are as follows
* We use 100 documents
* We use Tanh with random negative doc.rep. (no specific reason for the choice.)
* We train 50 epochs and save results after the end of epochs. 
* We run experiments with layers (0, 5, 10, 15) which covers all initial to final layers of `Pythia 1B`.
* Experiments requires roughly 1 hour 30 minutes. 



