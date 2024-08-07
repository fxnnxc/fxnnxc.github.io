---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-07-30
featured: true
tags: Neuron, Cognitive
img: /assets/kor/neurons_as_neurons/transmission.png
title: '뉴런 - 1. 신경망의 뉴런은  신경세포와 얼마나 닮았는가'
description: '종종 딥러닝의 뉴런은 신경세포에 비유된다. 수많은 뉴런들이 결정을 내리는 것이 신경세포와 닮았있기 때문이다. 그러나, 그 둘의 메커니즘이 어디까지 똑같은지 우리는 자세히 모른다. 이 글은 신경학적인 방식으로 딥러닝을 해석하는 글이다.'
toc:
  - name: 1. 소개
  - name: 2. 신경학적 뉴런 
    subsections:
      - name : 2-1 뉴런 
      - name : 2-2 시냅스
      - name : 2-3신경학적 신호전달 
  - name: 3. 딥러닝적 뉴런 
  - name: 4. 신호전달 방식 
    subsections:
      - name : 4-1 신호 전달 방식
      - name : 4-2 피드백 방식
      - name : 4-3 모듈
  - name: 5. 정보의 종류
    subsections:
      - name : 5-1 상태의 부재
      - name : 5-2 생물의 상태
  - name: 6. 결론
  
---

# 1. 소개

인간을 이해하기 위한 수많은 시도들은 생물학, 신경학, 동물행동학, 철학등 다양한 분야에서 이루어졌으며, 그 중에서 인지과학은 뇌에 존재하는 수많은 뉴런들이 생성하는 정보를 해석하는 방식으로 인간을 이해한다. 뉴런들은 정보를 처리하고, 의심의 여지없이 뉴런이 더 많으면 연결고리인 시냅스의 수가 더 많아지고 생물의 지능은 오른다. 그러나 인간의 수많은 뉴런들을 제대로 이해하는 것은 개별 뉴런을 조작할 수 없기에 쉽지 않았다.  그러나 딥러닝은 개별 뉴런들을 일일이 디버깅함으로써 인지적인 과정을 해석할 수 있게 되었다. 궁극적으로 인간의 뉴런들이 딥러닝의 뉴런들과 유사하다면, 인간의 인지 메커니즘을 이해하는데 도움을 줄 것이고, 반대로 인간을 좀더 이해한다면 딥러닝의 인지과정을 이해하는데 도움을 줄 것이다. 그러므로, 이 글은 두 분야의 뉴런을 비교함으로써 두 분야를 더 이해하려는 시도이다. 

딥러닝을 연구하는 내가 이 글을 적는 것은 신경학적 뉴런보다는 딥러닝적 뉴런에 대해서 고민하였고, 그들을 해석하는 방법이 **신경학적 뉴런의 매커니즘**을 제대로 이해하는 게 도움이 되는 것을 알게 되었기 때문이다. 두 가지는 서로 너무 닮아있지만, 분명한 차이를 지니고 있다. 이 글에서는 그들을 분석하여, 딥러닝과 신경학 모두에 뉴런을 바라보는 새로운 관점을 알리고자 한다. 신경학적 뉴런과 딥러닝적 뉴런이 얼마나 닮아있을까? 아니, 그 전에 **딥러닝의 뉴런이란 정확히 무엇일까?**

딥러닝 분야는 인간을 모델링 하는데 초점을 맞췄으며, 인간이 할 수 있는 것이 모델이 가능한지 여전히 궁금증으로 남아있다. 이러한 꿈으로부터 사람의 인지적 행동의 근원인 뉴런을 딥러닝에 비유한다. 그러나, 비유에 대한 근거는 사람의 뇌와 몸에 있는 수 많은 신경세포들이 딥러닝의 파라미터들과 유사하다는 것으로 정확한 뉴런의 정의가 애매하다. 적어도 내가 아는 수준에서 딥러닝적 뉴런의 정의는 명확하지 않은데, 왜냐하면 딥러닝 내부는 수많은 값들이 존재하며 정확히 어떤 부분을 뉴런이라고 칭해야 하는지 불분명하기 때문이다. 예를 들어서, 3개의 `Linear` 레어어를 쌓는 경우 정확히 어느 부분을 뉴런으로 칭해야 하는지 불명확하다<d-footnote> 딥러닝 모델은 `Weight` 와 `Activation` 함수 $\sigma$ 로 다음과 같이 표현할 수 있는데, 해당 부분에서 뉴런이 몇개인지 불명확하다.  $f(x) = \sigma(W_1\sigma(W_2\sigma(W_3 x)))$  </d-footnote>. 

딥러닝을 뉴런과 시냅스들로 이해하기 위해서는 먼저 신경학적 뉴런의 계산과정을 명확하게 파악하고, 이로부터 딥러닝이 신경세포와 **동일하게 계산하는 부분을 밝혀내는 과정**이 필요하다. **신경학적 뉴런**으로부터 **딥러닝적 뉴런**을 정의하고 두 뉴런 정의들간의 공통점과 차이점을 밝히며, 인간과 딥러닝이 얼마나 유사한지 살펴보자. 

# 2. 신경학적 뉴런 

인간의 몸에는 수많은 세포들이 있는데, 세포들 중에서 신경의 전달을 목적으로 하는 신경세포, `뉴런`이 있다. 뉴런들은 서로 의사소통을 하며 전기적인 신호가 전파되는데, 두 뉴런이 연결됨으로써 생기는 접합지점은 `시냅스`라고 한다<d-footnote> * 뉴런 : 개별 세포 <br> * 시냅스 : 두 개의 뉴런이 연결된 부분</d-footnote>. 인간의 뇌에는 약 8,600억개의 뉴런이 존재하며, 하나의 뉴런은 7,000 개의 다른 뉴런들과 연결되는 시냅스를 가진다. 따라서, 뇌에 존재하는 전체 시냅스의 수는 600조로 다음과 같이 계산할 수 있다. 

$$
\begin{gather}
\text{뉴런 수} \times \text{뉴런 당 시냅스 수} \approx \text{전체 시냅수 수} \\ 
\Rightarrow 8600 \text{억} \times 7,000 \approx 600 \text{조}
\end{gather}
$$

시냅스 수는 연결의 수이고, 정보를 담고 있는 것은 뉴런이다. 뉴런들이 특정한 대상에 대해서만 활성화가 된다면, 다음과 같은 뉴런의 활성화 연쇄작용을 생각할 수 있다. 

1. 망막으로부터 빛에 대한 정보를 입력받는다. (**강아지 입력** 🦮)  
2. 시신경세포들이 활성화되고 정보를 뇌에 전달한다. (**시신경 뉴런** 👀)
3. 뇌에 존재하는 세포들이 활성화됨으로써 여러가지 생각이 일어난다. 
  * 강아지 그림을 보고 강아지라는 단어를 떠올린다. (**강아지 뉴런** 🐶)
  * 강아지 단어가 떠올라 고양이 단어가 떠오른다. (**고양이 뉴런** 😸)
  * 강아지가 앞으로 움직일 방향을 예측한다. (**미래예측 뉴런** ⌛️)
  * 강아지에 대한 트라우마가 떠오른다. (**장기기억 뉴런** 🐾)
  * 트라우마가 떠올라 불편한 기분이 든다. (**감정 뉴런** 😢)

위 예시에서 1,2,3과 같은 방식으로 정보처리 회로를 구상하였는데, 이를 위해서는 뉴런들이 반드시 연결되어 있어야 한다. 이 연결을 책임지는 것이 `시냅스`이다. 입력으로는 단순히 강아지의 모습만 들어왔지만, 강아지 뉴런의 활성화는 더 나아가서 감정 뉴런까지 활성화시킬 수 있다. 이러한 연쇄적인 반응을 이해하기 위해서 뉴런이 어떤식으로 시냅스를 통해서 의사소통하는지 이해해보자. 

---

## 2-1 뉴런 

> 설명을 읽기 전에, 뉴런을 시각적으로 이해하는 것은 도움이 될 것 같아서 첨부.
<div>
<iframe width="340" height="315"
src="https://www.youtube.com/embed/SczOfOXY17U">
</iframe>
<iframe width="340" height="315"
src="https://www.youtube.com/embed/GIGqp6_PG6k">
</iframe>
</div>

### 기본 구조 



뉴런들이 서로 통신하는 방법은 **화학적 물질을 전달하여, 전기적 상태를 변경하는 방식**이다. 이를 이해하기 위해서 먼저, 뉴런이 어떤식으로 구성되는지 살펴보자. 뉴런은 다음과 같은 요소들로 구성되어 있다.


<figure style="text-align:center; display:block;" >
<img src="/assets/kor/neurons_as_neurons/neuron.png" style="width:100%">
<figcaption>
Figure. 뉴런의 기본 구조. 입력을 받는 부분인 dendrite과 전하를 이동하는 axon, 그리고 출력을 내보내는 axon terminal 로 이루어져 있다. 
</figcaption>
</figure>

* `Dendrites` : 다른 뉴런들로부터 화학물질을 받는 부분으로 송신을 맡는다.
* `SOMA` (Nuclear) : 뉴런의 핵
* `Axon` : 긴 허리로 `Dendrites` 와 `Axon Terminal` 을 연결하는 부위이다. 전기적 신호를 전달한다. 
* `Axon Terminals` : 다른 뉴런들로 화학물질을 전달하는 부분으로 발신을 맡는다. 

### 정보 전달 방법

뉴런은 기본적으로 음전하(Negative Ion) -70mv 를 띄고 있으며, `Dendrite` 에 $Cl^{-}$ 음이온이 전달될 경우, 그대로 음전하를 띈다. 
만일 `Dendrite`에 $Na^{+}$와 같은 양이온이 전달되는 경우, 음전하를 띄던 `Dendrite` 부분은 양전하를 띄게 된다. 그리고, 전하를 일정하게 유지하는 `Neutral Charge Equilibrium`  상태를 만족하기 위해서, 양전하는 음전하 방향으로 값이 전달된다. 이 때 지나는 통로를 `Axon`이라고 부르며, `Axon Terminal` 이 양전하 상태가 되어 화학물질을 내보내는 상태로 변경된다. 

<figure style="text-align:center; display:block; grid-column:middle;">
<img src="/assets/kor/neurons_as_neurons/neuron_state.png" style="width:180%">
<figcaption>
  뉴런이 전기적 신호를 dendrite에서 axon terminal 로 보내는 과정. 뉴런의 내부는 기본적으로 (-)를 띄는데, 활성화가 되는 부분은 상대적으로 (+)를 띈다. 
</figcaption>
</figure>


요약하자면, 뉴런이 다른 뉴런들로부터 화학물질을 받아서 양전하 상태가 되면 화학물질을 내보내고 결과적으로 연쇄적인 상태변화가 일어난다. 


> **(Option)** 전달하는 화학물질 
위에서 음이온과 양이온을 전달한다고 하였지만, 사실 정확하게는 다양한 종류의 화학물질이 있으며, 그 역할에 대해서 신경학에서는 더 자세히 다룬다. 여기서는 단순히 활성화 관점에서만 살피므로 필요한 내용은 아니다.
* Amino Acids (Most Functions)
  * Glutamate (가장 흔한) (**excitatory**)
  * GABA (Gamma-Aminobutyric acid) -  (brain)  (**inhibitory**)
  * Glycine  - (spine) (**inhibitory**)
* Monoamines (Attention, Cognition, Emotion)
  * Serotonin 
  * Histamine
  * Catecholamines
    * Dopamine
    * Epinephrine (Adrenaline)
    * Norepinephrine (Peripheral nervous system)
* Peptides (chain of amino acids) (Pain)
  * Opioids
    * Endorphin (Pain)
* Others 
  * Acetylcholine (Autonomic Nervous System, muscles)

## 2-2 시냅스

> ⚠️ 시냅스에 대한 이해는 뉴런의 의사소통을 명확하게 이해하는데 필요하지만, 딥러닝과 비교를 위해서 반드시 필요하진 않다. 요약하자면, **뉴런이 여러 뉴런들의 값을 종합하여 활성화를 결정하고 해당 뉴런은 다음 뉴런에 값을 전달한다는 것이다.**는 것이다. 이 장은 TMI일 가능성이 있어서, 불필요한 사람은 2-3 신경학적 신호전달체계로 넘어가도 충분하다. 

정보를 받는 뉴런에게는 Dendrite 가, 정보를 전달하는 뉴런에는 Axon Terminal 이 관여하며 이 두가지가 연결된 부분을 `시냅스`라고 부른다. 
따라서 시냅스는 두 개의 뉴런이 연결되어 발생하는 지점이며, 신호를 전달해주는 쪽과 받는 쪽을 구분하여 다음과 같이 부른다.  `N: 뉴런, [*] 뉴런에 연결되는 다른 뉴런들.` 
* **(좌) Presynaptic Neuron** : `Axon Terminal`로 신호를 전달 **하는** 뉴런  $(N \rightarrow \[*\])$
* **(우) Postynaptic Neuron** : `Dendrite`으로 신호를 전달 **받는** 뉴런 $$([*] \rightarrow N)$$

<figure style="text-align:center; display:block;width:100;" >
<img src="/assets/kor/neurons_as_neurons/synapse.png" style="width:100%">
<figcaption>
  Figure. 두 뉴런과 이들의 연결로 인해서 생기는 시냅스. 신호를 주는 쪽은 axon terminal을 통해서, 받는 쪽은 dendrite을 통해서 통신한다. 
</figcaption>
</figure>

### 여러 뉴런으로부터 의견 취합

하나의 뉴런이 전달하는 전기적 상태는 단순히 (+) 혹은 (-) 이지만 뉴런이 받는 화학물질은 여러 개의 뉴런들이 보내는 정보가 `Dendrites` 에 취합된다. 따라서, 여러 뉴런의 전기적 신호의 합이 최종적으로 `Axon` 을 통해서 전달하는 전기적 신호로 생각될 수 있다. 이는 각 Dendrite 들이 전달받은 음이온 또는 양이온의 신경전달물질 (Neurotransmitters) $T_1, T_2, \cdots$ 로부터 전기적 신호가 결정됨을 나타낸다. 

$$
\text{전기적 신호} = T_1 + T_2 + T_3 + T_4 + \cdots
$$

<figure style="text-align:center; display:block;width:100;" >
<img src="/assets/kor/neurons_as_neurons/transmission.png" style="width:100%">
<figcaption style="text-align:left">
  Figure. (위) 뉴런의 Dendrites 에 여러 개의 뉴런이 신호를 보내고, 최합된 정보는 전기적 신호로 우측에 도달한다. (아래) Axon Terminal이 양전하 상태가 되면 화학물질들을 내보내고 연결되어 있는 뉴런들에 화학물질이 들어간다. 이는 각 뉴런들의 전하를 변경하는 역할을 한다. 
</figcaption>
</figure>


결과적으로 뉴런의 상태인 전기적 신호는 **무조건 1개**이다. 그리고 이 상태는 여러 개의 `Axon Terminal`에 영향을 미치므로, 전파된 정보는 여러 뉴런들에 연결되어 전파된다. 전달받은 신경전달물질의 전기적 상태에 대해서 3가지 가능성이 존재한다. 
 1. **모두 양이온들이라면, 뉴런은 활성화된다. (+)** 
 2. **모두 음이온들이라면, 뉴런은 비활성화된다. (-)**  
 3. **만일 음이온과 양이온이 적절하게 섞여있다면, 활성화될수도 안될수도 있다. (+ Or -)** 

연결되어 있는 뉴런이 전달하는 신경전달 물질의 상태에 대해서 받는 쪽 뉴런을 **활성화시키는지, 억제시키는지에 따라서 물질을 구분할 수 있다.** 이러한 뉴런이 전달하는 물질의 구분은 딥러닝의 뉴런을 **Excitatory** 와 **Inhibitory**로 구분한 **Thread: Circuits** <d-cite key="cammarata2020thread"/> [[post](https://distill.pub/2020/circuits/)] 에서도 나타나는 개념으로, 뉴런들간의 관계를 파악하는데 아주 중요한 역할을 한다. 

> **흥분과 억제 신경전달물질, Excitatory and Inhibitory Neurotransmitters**
* 🔥 흥분 신경전달물질 (Excitatory Neurotransmitters)
  * Na+ 
  * Acetylcholine
  * Noradrenaline 
  * Glutamate 
* ❄️ 억제 신경전달물질 (Inhibitory Neurotransmitters)
  * Glycine
  * GABA (gamma-aminobutyric acid)
  * Cl-

<figure style="text-align:center; display:block;width:100;grid-column:middle;">
<img src="/assets/kor/neurons_as_neurons/excitation_and_inhibition.png" style="width:120%">
<figcaption>
  Excitatory Neuron 은 다음 뉴런을 흥분시키는 역할을 하고, Inhibitory Neuron 은 다음 뉴런을 억제시키는 역할을 한다. 
</figcaption>
</figure>

## 2-3  신경학적 신호전달체계

사람은 수많은 인지과정을 거치므로 정확히 어떤 뉴런이 어떤 역할을 가지는지, 현재 뇌는 어떤 정보를 처리하고 있는지 명확하지 않다. 또한, 뉴런들의 값을 모두 추적하기 위해서도 많은 현대기술들이 필요하다. 그러나 이와 다르게 딥러닝 모델들은 입력에 대해서 딥러닝 모델의 뉴런들이 어떤식으로 반응하는지 값을 추적할 수 있으며, 반응을 확인할 수 있다. 또한 뉴런들간의 연결이 존재한다면 이를 정량적으로 디버깅할 수 있다. 만일 딥러닝의 뉴런이 신경세포의 통신과정과 비슷하게 정보를 저장하고 전달한다면, 딥러닝의 뉴런을 분석하는 것이 신경학의 뉴런을 분석하는 열쇠가 될 수 있다. 그런데, *딥러닝에서 뉴런이라는 것은 정확하게 뭘까?* 이에 대한 답을 내려보고자 한다. 

---

# 3. 딥러닝적 뉴런 

딥러닝 모델의 내부에 대해서 가지고 있는 생각은 대부분 다음과 같을 것이다.  

> 수많은 파라미터들이 레어이마다 존재하며, 입력과 출력을 End-to-End 로 계산하는 블랙박스 모델 

모델 내부를 분석하는 것은 너무 어려운 일이며, 내부에서 어떤 일이 일어나는지, 파라미터들이 어떤 역할이 있는지 명확하지 않다. 
그러나 딥러닝적 뉴런에 대한 이해한다면, 모델의 내부를 뉴런들의 통신으로 생각할 수 있고 딥러닝 모델에 대해서 다음과 같은 관점이 생긴다. 

> 각 레이어마다 뉴런들이 독립적으로 입력의 활성화 정도를 계산하는 신경학적 구조 

## 뉴런과 시냅스

신경학적 뉴런에서 하나의 뉴런은 여러개의 `Dendrite` 을 가져서 정보를 받고, 출력으로 하나의 전기적 신호가 생긴다고 설명하였다. 
이 과정은 딥러닝에서 입력벡터에 대해서 파라미터를 내적을 계산하는 것과 동일한 방식이다. 

<figure style="text-align:center;">
<img src="/assets/kor/neurons_as_neurons/neuron_as_neuron.png" style="width:100%">
<figcaption>
  Figure. 신경학적 뉴런과 딥러닝 뉴런의 연산의 유사성. 신경학적 뉴런이 전하를 바꾸는 neurotransmitter들로 인해서 출력이 생기는 것과 유사하게, 딥러닝적 뉴런도 입력들을 결합하여 전하상태를 내보낸다.
</figcaption>
</figure>


신경학적 뉴런의 역할이 신호들을 결합하여, 새로운 신호를 내보내는 것으로 고려한다면 딥러닝에서 `Linear Sum` 을 하는 Weight들의 결합이 뉴런이다. 따라서 딥러닝적 뉴런은 다음과 같이 정의할 수 있다. 

> **🚀 신경학적 뉴런으로부터 도출된 딥러닝적 뉴런의 정의** <br>
**딥러닝적 뉴런** : Weight 들의 집합 (입력에 대해서 결합을 하는 Weight) <br>
**딥러닝적 시냅스** : 개별 Weight

위에서 사람의 시냅스 개수는 600조 개수라고 하였는데 (⭐️), GPT3 의 파라미터 개수는 1750억개이다. 따라서, GPT3 가 사람보다 시냅스의 개수가 더 적을 것을 알 수 있다. 딥러닝적 뉴런의 정의를 바탕으로 실제 딥러닝 모듈들에서 몇 개의 뉴런과 시냅스가 존재하는지 살펴볼 수 있는데, 가장 기본이되는 Linear Weight 에 대해서 해석해보자<d-footnote> 더 복잡한 모듈들인 Multi-Layer Perceptron (MLP), Convolutional Neural Network (CNN) 그리고 GPT3 모델의 뉴런들에 대한 분석은 [뉴런 - 4. 다중 뉴런 분석] 을 참고.
</d-footnote>. 먼저, 신경학적 뉴런은 `K`개의 화학적신호를 바탕으로 `1`개의 전기적 신호를 만들어낸다<d-footnote> 1 개의 전기적 신호는 다시 결합되어 있는 다른 뉴런들에게 화학물질을 전달하는 역할을 한다. 입력과 출력의 관점에서 신경학적 뉴런은 여러개의 입력과 출력을 가진다. 그리고 여러 개의 출력은 다시 여러 개의 뉴런들과 연결되는데, 이 과정에서 출력과 다음 입력은 서로 맞물려서 결과물을 내는 것으로 생각할 수 있다. 이 때, 결과물을 출력하는 쪽이 아니라, 입력으로 받는 쪽에서 처리하는 것으로 고려한다면, 뉴런의 출력은 단 1개로 생각할 수 있다</d-footnote>.

* 신경학적 뉴런의 연산
  * (In) **뉴런이 받은 K개의 신호들**: $a_1, a_2, a_3, \cdots, a_k $ (플러스 정도, 뉴런이 음전하 상태이면 0)
  * (Weight) **뉴런의 K개 Dendrite**: $w_1, w_2, w_3, \cdots, w_k $ (내보내는 신경전달물질 음~양전하)
  * (Out) **신경전달 물질을 받는 뉴런의 활성화 정도** : $\sum_k w_k \cdot a_k$

* 딥러닝적 뉴런의 연산 (`Weight` 와 `Activation` 내적)
  * (In) **뉴런 파라미터** :  $w_1, w_2, w_3, \cdots, w_k $
  * (Weight) **뉴런 입력** : $a_1, a_2, a_3, \cdots, a_k $ 
  * (Out) **뉴런 출력** : $\sum_k w_k \cdot a_k$

여기서 선형레이어가 가지는 두 가지 Weight $W\in \mathbb{R}^{N\times K}$과 Bias $\mathbf{b}\in \mathbb{R}^{N}$를 해석하면 $K$개의 뉴런이 있으며, 각 뉴런은 $K$ 개의 신호를 종합하여 1개의 아웃풋을 내보낸다. 총 $N$개의 뉴런들이 신호를 1개씩 내보내므로, 결과적으로 $N$ 차원의 출력이다. 

$$
\begin{gather}
\mathbf{y} = W\mathbf{x} + \mathbf{b}  \\
\begin{bmatrix}
  y_1 \\ 
  y_2 \\
  \vdots \\ 
  y_N
\end{bmatrix}
=
\begin{bmatrix}
  w_{11} & w_{12}& \cdots & w_{1K} \\ 
  w_{21} & w_{22}& \cdots & w_{2K} \\ 
  \vdots \\ 
  w_{N1} & w_{N2}& \cdots & w_{NK} \\ 
\end{bmatrix}
\cdot 
\begin{bmatrix}
  x_1 \\ 
  x_2 \\
  \vdots \\ 
  x_K
\end{bmatrix}
+ 
\begin{bmatrix}
  b_1 \\ 
  b_2 \\
  \vdots \\ 
  b_N
\end{bmatrix}
\end{gather}
$$

* ⭐️ 뉴런의 개수 : $N$ (Dimension of Output)
* ⭐️ 시냅스의 개수 : $N\times K$ (Number of Weights)

결국 딥러닝의 뉴런은 입력을 해석해서 아웃풋을 내보내는 weight 에 대한 group 으로 정의가 된다. 딥러닝에는 Linear가 아닌 다양한 모듈들이 존재하며 이에 대한 해석도 Linear를 해석한 것처럼 가능하다. 그러나 이 글의 목적은 딥러닝과 신경학적인 뉴런을 비교하는 것이므로 해당 선형 뉴런을 바탕으로 신경학적인 특징을 비교해보자. 

---

# 4. 신호전달 방식

딥러닝적 뉴런은 신경학적 뉴런과 유사한 방식으로 정보를 처리하며, 유사점으로 수많은 뉴런들이 서로 연결되어 정보를 주고 받는 것까지 유사하다. 
그러나 단순히 연산의 유사성을 넘어서 뉴런들이 학습하고 통신하는 방식에는 차이점이 존재하고 이로부터 딥러닝은 본질적으로 인간이 지니는 특징을 가지지 못할 수 있다. 
딥러닝적 뉴런의 학습에는 두 가지 연산이 필요한다. `정보를 전달하는 forward` 와 `피드백을 받는 backward` 이다. 딥러닝 모델들은 명확하게 forward 로 계산한 결과에 대해서 피드백을 받고, 이를 backward로 전달시킨다. 이로부터 뉴런의 파리미터들이 변화하면서 뉴런 간의 forward 구조가 바뀌는 것이다. 전반적인 학습을 관장하는 두 가지 전파 forward 와 backward에 대해서 좀더 자세하게 알아보자. 

<h2 id="4-1-신호-전달-방식"> 4-1 신호 전달 방식 (Forward) </h2> 

딥러닝 뉴런의 경우 forward는 서로 연결되어 있는 뉴런들끼리 전달하며, 일반적으로 한 번 연산을 거친 뉴런은 다시 연산하지 않는다. 하나의 뉴런이 두 번 이상의 연산을 하게 된다면, 이는 `parameter sharing` 으로 해석되는데, 대부분은 파라미터 수를 줄이기 위해서 사용된다. RNN의 경우 뉴런이 연속적으로 정보를 처리하는 것으로 생각할 수 있지만, 이는 뉴런들간의 커뮤니케이션에 대한 결과라기보다는 순차적인 입력과 잠재표현을 순환적으로 처리하는 것이다<d-footnote> RNN에서 두 번 이상의 연산을 거치지만 이는 연속적인 데이터를 연속적으로 처리하는 것이므로 단일 입력에 대한 복수 개의 연산으로 해석하지 않았다. RNN의 경우, N개의 입력에 대해서 뉴런이 N번 연산하며, Parameter Sharing은 1개의 입력에 대해서 여러 번 계산한다.</d-footnote>.  따라서 뉴런들은 보통 하나의 정보를 처리하도록 설계되었다. 

또 따른 차이점은 딥러닝적 뉴런의 커뮤니케이션 방향이 단방향 ($\rightarrow$) 라는 것이다. 그들의 커뮤니케이션은 순차적인데, 좀더 명확하게 순차적인 이유는 그들의 axon terminal 과 dendrite 이 단일 방향으로 구성되었기 때문이다. 그러나 신경학적 뉴런의 경우 axon terminal 과 dendrite 이 동일한 방향으로 정렬되어 있는지는 미지수이다. 이는, 연산이 반드시 한쪽 방향으로 이루어지지 않을 수 있다는 것이다. 아래 그림처럼 뉴런들이 45도 각도로 구성되어있다면, 본인이 내보낸 시그널은 7개의 뉴런들을 거쳐서 다시 본인에게 돌아온다. 이러한 현산이 가능한 이유는 그들의 신호전달매체(axon terminal 과 dendrite)가 일렬로 정렬되어 있지 않기 때문이다. 

<figure style="text-align:center; display:block;width:100;">
<img src="/assets/kor/neurons_as_neurons/communication.png" style="width:100%">
<figcaption>
  Figure. 신경학적 뉴런과 딥러닝적 뉴런이 커뮤니케이션을 하는 방법. 신경학적 뉴런은 axon terminal 와 dendrite 의 연결이 일자가 아니므로, 본인의 시그널이 다시 돌아올 수 있다. 반면에 딥러닝의 Computational Graph 는 순차적인 계산이고, 뉴런들이 정렬되어 있다. 
</figcaption>
</figure>

<h2 id="4-2-피드백-방식"> 4-2 피드백 방식 (Backward) </h2> 

뉴런들은 지속적으로 업데이트 되며, 이러한 업데이트에는 외부자극이 큰 영향을 미친다. 가장 큰 뉴런 업데이트 차이는 `파라미터 변화의 원인`이 다르게 때문이다. 딥러닝의 파라미터 변환은 대부분 `loss`에 대한 gradient descent로부터 이루어지나, 신경학적 뉴런은 반드시 `loss`가 존재하지 않을 수 있다. 물론 사람은 피드백으로부터 행동을 업데이트 하며, 점차적으로 뉴런들이 행동에 대해서 강화된다. 그러나, 이는 필요조건이 아니며 시그널이 없어도 뉴런은 변화할 수 있다. 예를 들어서, 그들의 통신에 가장 큰 영향을 미치는 화학적 물질이 결핍되면 뉴런들의 연결은 약해지고 계산 결과가 바뀌게 된다. 또한 신호전달로 인해서 화학물질들이 줄어들 수 있으므로, 단순히 `forward` 과정으로 인해서 뉴런들 상태가 변한다고 볼 수 있다. 그러나, 딥러닝 모델은 forward 과정에서 파라미터들이 **고정**이고 오직 `backward`로 인해서 뉴런들의 정보가 변화한다. 

인간의 학습하는 가장 효율적인 방법은 반복하는 것으로, 연습을 통해서 새로운 테스크에 적응하는 능력이 뛰어나다. 이러한 과정은 사용한 뉴런을 강화하는 것으로 근육을 단련하는 것처럼 연결을 강화하는 것이다. 따라서, 뉴런은 반복적인 `forward` 과정으로부터 학습한다. 이에 대한 근거로 딥러닝의 뉴런은 axon terminal 과 dendrite 모두 시그널을 전파하는 역할을 하지만, 신경학적 뉴런은 명확하게 한 방향으로만 정보가 전달된다는 것을 들 수 있다. 이러한 직감을 잘 반영한 연구는 Geoffrey Hinton [[scholar](https://scholar.google.com/citations?user=JicYPdAAAAAJ&hl=ko&oi=ao)] 의 forward-forward algorithm <d-cite key="hinton2022forward"/> 으로 `생물이 오직 forward 전파를 통해서` 학습이 된다는 사실을 반영하여 모델링을 제안하였다. 


<h2 id="4-3-모듈"> 4-3 모듈 </h2> 

인간의 뇌에 대한 가장 큰 특징 중 하나는 `뇌의 영역마다 서로 다른 역할`을 하는 뉴런들이 있다는 것이다. 신경세포가 본인이 포함되어 있는 그룹에 따라서 역할이 달라지며, 이는 인간이 지닌 다양한 면을 해석하는데 도움을 준다. 예를 들어서 변연계는 보다 감성적인 역할을 하는 것으로 알려져있으며 전두엽은 논리와 계산과 같은 과정을 담당한다. 또한 장기기억은 뇌의 안쪽에 저장되며, 단기기억은 전두엽 부분에 저장된다. 뉴런들은 동일한 생물체일지라도 그 위치에 따라서 본인이 처리하는 정보가 서로 다르다. 딥러닝이 이러한 모듈화를 진행하는지는 수수께끼로 남아있다. 물론 뉴런을 역할을 명시적으로 구분한 *Inductive Bias*를 추가하는 경우, 그 뉴런의 역할을 우리가 원하는 대로 설계할 수 있다<d-footnote>예를 들어서, CNN의 필터 사이즈를 바꾸는 것은 이미지의 전혀 다른 특징을 포착하도록 만들 수 있다.</d-footnote>. 최근에 밝혀진 바로는 명확하게 모듈을 설계하는 것이 아닌, `회로를 그룹별로 묶는 것이 모듈화를 진행`한다는 연구가 있다. Branch Specialization<d-cite key="voss2021branch"/> [[link](https://distill.pub/2020/circuits/branch-specialization/)] 연구는 두 개의 서로 다른 브랜치에서 학습된 뉴런들은 각 그룹에서 비슷한 특징을 가지게 된다는 사실을 보여줬다. 

<figure style="text-align:center; display:block;width:100;">
<img src="https://distill.pub/2020/circuits/branch-specialization/images/Figure_1.png" style="width:70%">
<figcaption>
Figure. 명시적으로 나뉜 두 개의 브랜치의 뉴런들이 어떤 정보를 담고 있는지 보면, 브랜치 내 뉴런들의 정보가 유사한 것을 볼 수 있다. 이러한 결과는 우연이라고 보기 어려운 결과이다. 
  © Image From branch specialization [<a href="https://distill.pub/2020/circuits/branch-specialization/" > link </a>]
</figcaption>
</figure>

# 5. 정보의 종류

딥러닝이 인간과 비교했을 때 가지는 가장 불공평한 것은 입력의 차이이다. 
사람은 눈은 매 순간 정보를 입력으로 받는데, 1초에 200장 정도의 이미지를 높은 해상도로 받고 있다. 딥러닝은 빈약하게 이미지 한장 정도의 수준의 입력을 받는다. GPT 모델에게 이후 텍스트를 생성해달라고 할 때, 굉장히 적은 길이의 입력을 넣는 것으로 긴 문장을 생성하는 것은 주어진 상황에 대한 정보량이 인간과 딥러닝이 차이가 심하다는 것을 상기시킨다. 최근에 나온 GPT4 <d-cite key="openai2023gpt4"/> 나 GPT3.5는 굉장히 긴 문장을 입력으로 받기도 하는데, 이는 점점 인간이 입력으로 받는 정보량에 딥러닝이 근접한다는 것을 나타낸다. 요약하자면, 예전에는 딥러닝 모델이 하나의 입력에 대해서 적응을 잘하는 목표로 연구했다면, 최근에는 좀더 다양하고 많은 입력을 모델에게 연산시킨다. 그러나, `딥러닝의 가장 큰 단점은 입력이 주어져야 반응`한다는 것이다. 바꿔말하면, *입력이 없다면 딥러닝은 무엇을 할 수 있는가?* 


## 5-1 상태의 부재

`인간은 입력이 주어지지 않아도` 뉴런들이 반응하고 정보를 처리할 수 있다. 예를 들어서, 머릿속으로 사진을 하나 상기시키고 해당 사진으로부터 결론에 도달하거나 새로운 생각을 만들어낼 수 있다. 이러한 정보는 뇌의 장기기억에 보관된 정보들을 꺼내는 과정으로 컴퓨터로 치면 하드디스크에 저장된 정보를 꺼내는 것이다. 그러나, 딥러닝 파라미터는 주어진 입력에 대한 연산을 진행하도록 구성될 뿐, 메모리가 부재하다. 물론 Neural Turing Machine<d-cite key="graves2014neural"/>처럼 메모리를 명시적으로 사용한다면 가능하지만, 이들의 반응성 또한 입력에 의존적으로 이루어진다<d-footnote>정확히는 입력에 따라서, 적절한 메모리를 고르는 것이다. 따라서 입력에 의존적이다. </d-footnote>. 


## 5-2 생물의 상태

또 다른 차이점은 입력의 종류이다. 딥러닝 모델은 입력으로 받는 공간이 고정되어 있으며, 시각적인, 언어적인, 기호적인, 음성적인 정보들이 입력 레이어를 통해서 들어온다. 사람으로 비유하면 눈과 귀를 통해서 정보를 전달받는 것이다. 따라서 딥러닝이 처리하는 정보는 오직 입력과 내부 뉴런들의 연산 방법에 의해서 처리된 결과물이다. 그러나, 사람이 뉴런을 활성화하는 과정에는 `입력으로 받는 정보 이외에 추가적인 정보가` 있다. 바로 `몸` 또는 `인지`의 상태이다. 

### 몸의 상태
> 딥러닝은 피곤하다고 계산 결과를 바꾸는가? 아니요. 

인간은 동일한 입력을 받을지라도, 현재 `호르몬 분비에 의해서 처리결과는 바뀔 수 있는데`, 본인의 감정이 슬프다면, 동일한 사진을 봐도 다른 해석이 가능한 것이다. 이는 몸의 호르몬으로부터 뉴런의 처리가 달라질 수 있다는 것을 암시한다. 피곤할 때 의사결정을 내리는 것을 조심해야 하는 이유도 뉴런의 상태가 달라지기 때문이고, 뉴런의 상태가 최적이 아니라면 연산의 결과가 크게 달라질 수 있기 때문이다. 딥러닝 모델 파라미터에 노이즈가 많이 껴있는데, 중요한 의사결정을 하는데 사용하면 안되는 것처럼 말이다. 


### 인지의 상태
> 딥러닝은 이전에 본 정보를 계속 생각하는가? 아니요. 

또 다른 변수는 뇌가 현재 처리하고 있는 정보에 따라서 결과가 달라질 수 있다는 점이다. 대표적으로 `"인지적 대조 원리"`를 예로 들 수 있다. 인지적 대조 원리는 먼저 본 정보에 의해서 두 번째로 본 정보가 왜곡되어 해석되는 원리이다. 동물의 크기를 예로 든다면, 코끼리 옆에 있는 호랑이를 작다고 인지하게 되는데, 이는 코끼리가 크기 때문이다. 과연 딥러닝 뉴런은 이러한 인지적 대조 원리를 기반으로 사물을 분석할 수 있는지 확신할 수 없다. 또 다른 예로 연봉을 8000을 받다가 5000으로 줄어드면 적게 받는 것으로 사람은 인지하는데, 딥러닝 모델이 연봉 5000이라는 정보를 주어졌을 때, 단순히 5000이라는 정보에 의존적인 해석을 내릴 가능성이 높다. 


## 6. 결론 

딥러닝과 인간을 비교하는 것은 두 분야에 모두 긍정적으로 작용한다. 딥러닝 모델을 설계하면서 인지적인 영감으로 모델링 및 해석을 진행한 연구들은 셀 수 없이 많으며 <d-cite key="wijmans2023emergence"/>, 반대로 인간이 인사이트를 얻기 위해서나 뇌를 이해하고 위해서 딥러닝을 활용하는 것 또한 적지 않다. 두 분야 모두 인지적인 부분을 다룰 수 있기 때문이다. 이 글을 쓰면서, 뇌를 이해하는 것은 딥러닝을 마치 생물로 이해하는 것처럼 도움이 되었는데, 보다 복잡한 모델의 내부를 명시적인 뉴런들로 해석하여 딥러닝에 대하여 더 많이 이해했던 것 같다. 

뉴런 뿐만 아니라 딥러닝과 사람은 실제로 `더 많은 부분에서 유사점`이 존재할 것 같다. 예를 들어서, 사람은 학습을 위해서 유년기 시절을 보내고 시행착오를 겪고 나면 성인이 되어서 많은 것들을 빠르게 배울 수 있으며, 별다른 노력없이 건설적인 일들을 할 수 있다. 비슷하게 딥러닝 모델들로 학습을 위해서는 수 많은 데이터셋과 학습시간이 필요하지만, 하나의 연산을 위해서 사용하는 에너지는 적다. 이러한 배움에서 유사점도 그 부분을 명확하게 이해한다면 배움이라는 것을 이해하는데 또다른 도움을 줄 수 있을 것으로 보인다. 그리고, 딥러닝이 배우는 방식으로부터 사람이 배우는 과정을 더욱 견고하고 빠르게 만들 수 있을 것으로 보인다. `딥러닝은 배움을 다루는 분야`이고, 개인적으로 딥러닝이 배우는 방식을 통해서 개인적인 공부 방식에도 큰 도움이 되었던 것 같다<d-footnote> 예를 들어서, 강화학습은 순간적인 보상보다 장기적인 보상까지 고려해서 행동하라고 알려주는데, 이러한 사고방식은 오늘 무엇을 공부할지 결정하는데 도움을 줬다.</d-footnote>.  

수많은 과학적인 발견들은 비유를 통해서 발견된다. 자연으로부터 모방은 생물이 진화하고 적응하는데 있어서 중요한 역할을 하였다. 딥러닝의 뉴런이 생물학적인 뉴런과 얼마나 비슷할지 그 끝은 알 수 없지만, 지속적으로 모방하는 것이 인지과학, 신경과학, 딥러닝 분야들이 발전하는 가장 빠른 방법이라는 생각이 든다. 

--- 

## Acknowledgement 

I especially thank Yeonjea Kim for the ideas on the neurons in neuroscience 🙂


## 몇 가지 궁금증 

1. **뉴런이 적어도 인지가 가능한가?** 닭도 뉴런은 있으나, 그 수가 적을 뿐이다. 사람은 뉴런이 많이 있다. 따라서 인지의 정도는 뉴런의 개수로 나뉠 수 있는 것인가? 

2. 사람처럼 딥러닝의 뉴런들은 그래프 형태로 정렬 되어야 할까? (From *Yeonjea Kim*)
