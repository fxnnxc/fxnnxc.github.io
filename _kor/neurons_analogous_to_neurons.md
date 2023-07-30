---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-07-23
featured: true
img: /assets/kor/neurons_as_neurons/transmission.png
title: '뉴런 - 1. 신경망의 뉴런은 얼마나 신경세포와 닮았는가'
description: '종종 딥러닝의 뉴런은 신경세포에 비유된다. 수많은 뉴런들이 결정을 내리는 것이 신경세포와 닮았있기 때문이다. 그러나, 그 둘의 메커니즘이 어디까지 똑같은지 우리는 자세히 모른다. 이 글은 신경학적인 방식으로 딥러닝을 해석하는 글이다.'
toc:
  - name: 1. 소개
  - name: 2. 신경학적 뉴런 
    subsections:
      - name : 2-1 뉴런 
      - name : 2-2 시냅스
      - name : 2-3신경학적 신호전달 
  - name: 3. 딥러닝적 뉴런 
  - name: 4. 신호전달 방식 비교 
    subsections:
      - name : 4-1 신호 전달 방식
      - name : 4-2 피드백 방식
  - name: 5. 결여된 인지방식
---

# 1. 소개

인간을 이해하기 위한 수많은 시도들은 생물학, 신경학, 동물행동학, 철학등 다양한 분야에서 이루어졌다. 
그 중에서 인간의 인지과정을 이해하는 것은 인간이 고차원적인 의식을 가지고 있다는 믿음하에 뇌에 존재하는 수많은 뉴런들의 역할을 찾는 방식으로 신경학적 분석이 가능했다. 
의심의 여지없이 뉴런이 더 많으면 연결고리인 시냅스의 수가 더 많아지고 생물의 지능은 오른다. 그러나 수많은 뉴런들을 제대로 이해하는 것은 뉴런 간의 조합이 너무 많기 때문에 쉽지 않다. 그럼에도 불구하고 신경세포들을 이해하는 것은 인간이 가지고 있는 메커니즘을 이해하는데 도움을 준다. 

딥러닝을 연구하는 내가 이 글을 적는 것은 신경학적 뉴런보다는 딥러닝적 뉴런에 대해서 고민하였고, 그들을 해석하는 방법이 **신경학적 뉴런의 매커니즘**을 제대로 이해하는 게 도움이 되는 것을 알게 되었기 때문이다. 두 가지는 서로 너무 닮아있지만, 분명한 차이를 지니고 있다. 이 글에서는 그들을 분석하여, 딥러닝과 신경학 모두에 뉴런을 바라보는 새로운 관점을 알리고자 한다. 신경학적 뉴런과 딥러닝적 뉴런이 얼마나 닮아있는지, 어떠한 차이점이 딥러닝 모델과 유기체를 구분하는지 살펴보자. 

딥러닝 분야는 인간을 모델링 하는데 초점을 맞췄으며, 인간이 할 수 있는 것이 모델이 가능한지 여전히 궁금증으로 남아있다. 이러한 꿈으로부터 사람의 인지적 행동의 근원인 뉴런을 딥러닝에 비유한다. 그러나, 비유에 대한 근거는 사람의 뇌와 몸에 있는 수 많은 신경세포들이 딥러닝의 파라미터들과 유사하다는 것으로 정확한 뉴런의 정의가 애매하다. 적어도 내가 아는 수준에서 딥러닝적 뉴런의 정의는 명확하지 않은데, 왜냐하면 딥러닝 내부는 수많은 값들이 존재하며 정확히 어떤 부분을 뉴런이라고 칭해야 하는지 불분명하기 때문이다. 예를 들어서, 3개의 `Linear` 레어어를 쌓는 경우 정확히 어느 부분을 뉴런으로 칭해야 하는지 불명확하다<d-footnote> 딥러닝 모델은 `Weight` 와 `Activation` 함수 $\sigma$ 로 다음과 같이 표현할 수 있는데, 해당 부분에서 뉴런이 몇개인지 불명확하다.  $f(x) = \sigma(W_1\sigma(W_2\sigma(W_3 x)))$  </d-footnote>. 

딥러닝을 뉴런과 시냅스들로 이해하기 위해서는 먼저 신경학적 뉴런의 계산과정을 명확하게 파악하고, 이로부터 딥러닝이 신경세포와 **동일하게 계산하는 부분을 밝혀내는 과정**이 필요하다. 
이 글은 **신경학적 뉴런**으로부터 **딥러닝적 뉴런**을 정의하고, 두 뉴런 정의들간의 공통점과 차이점을 밝히며, 딥러닝 내부를 해석하는 다양한 용어들에 대해서 그 의미를 제시한다. 

# 2. 신경학적 뉴런 

인간의 몸에는 수많은 세포들이 있는데, 세포들 중에서 신경의 전달을 목적으로 하는 신경세포, `뉴런`이 있다. 뉴런들은 서로 의사소통을 하며 전기적인 신호가 전파되는데, 두 뉴런이 연결됨으로써 생기는 접합지점은 `시냅스`라고 한다. 
* `뉴런` : 개별 세포.
* `시냅스` : 두 개의 뉴런이 연결된 부분 
인간의 뇌에는 약 8,600억개의 뉴런이 존재하며, 하나의 뉴런은 7,000 개의 다른 뉴런들과 연결되는 시냅스를 가진다. 따라서, 뇌에 존재하는 전체 시냅스의 수는 다음과 같이 계산된다. 
$$
\begin{gather}
\text{뉴런 수} \times \text{뉴런 당 시냅스 수} \approx \text{전체 시냅수 수} \\ 
\Rightarrow 8600 \text{억} \times 7,000 \approx 600 \text{조} \\ 
\Rightarrow  8.6 \cdot 10^{12} \times 7 \cdot 10^3 \approx 6.0 \cdot 10^{15}  \text{(⭐️)}
\end{gather}
$$

(⭐️) 이 개수를 기억하자. 나중에 GPT3 <d-cite key="brown2020language"/> 랑 사람의 시냅스 수를 비교하기 위해서!

---

신경세포들은 화학적 물질들을 통해서 전기적 상태가 바뀌고 화학물질을 내보냄으로써 연결되어있는 신경세포들에게 통신을 하는 방식이다. 
인간 신경의 30% 이상을 차지하는 시신경으로 처리과정을 생각해보면, 다음과 같은 예시를 생각할 수 있다. 

1. 망막으로부터 빛에 대한 정보를 입력받는다. (**강아지 입력** 🦮)  
2. 시신경세포들이 활성화되고 정보를 뇌에 전달한다. (**시신경 뉴런** 👀)
3. 뇌에 존재하는 세포들이 활성화됨으로써 여러가지 생각이 일어난다. 
  * 강아지 그림을 보고 강아지라는 단어를 떠올린다. (**강아지 뉴런** 🐶)
  * 강아지 단어가 떠올라 고양이 단어가 떠오른다. (**고양이 뉴런** 😸)
  * 강아지가 앞으로 움직일 방향을 예측한다. (**미래예측 뉴런** ⌛️)
  * 강아지에 대한 트라우마가 떠오른다. (**장기기억 뉴런** 🐾)
  * 트라우마가 떠올라 불편한 기분이 든다. (**감정 뉴런** 😢)

위 예시에서 1,2,3과 같은 방식으로 정보처리 회로를 구상하였는데, 이를 위해서는 뉴런들이 반드시 연결되어 있어야 한다. 이러한 연결을 `시냅스`라고 부른다. 
입력으로는 단순히 강아지의 모습만 들어왔지만, 강아지 뉴런의 활성화는 더 나아가서 감정 뉴런까지 활성화시킬 수 있다. 이러한 연쇄적인 반응을 이해하기 위해서 뉴런이 어떤식으로 시냅스를 통해서 의사소통하는지 이해해보자. 


## 2-1 뉴런 

### 기본 구조 

<div>
<iframe width="340" height="315"
src="https://www.youtube.com/embed/SczOfOXY17U">
</iframe>
<iframe width="340" height="315"
src="https://www.youtube.com/embed/GIGqp6_PG6k">
</iframe>
</div>


뉴런들이 서로 통신하는 방법은 **화학적 물질을 전달하여, 전기적 상태를 변경하는 방식**이다. 이를 이해하기 위해서 먼저, 뉴런이 어떤식으로 구성되는지 살펴보자. 
뉴런은 다음과 같은 요소들로 구성되어 있다.


<figure style="text-align:center; display:block;" >
<img src="/assets/kor/neurons_as_neurons/neuron.png" style="width:100%">
<figcaption>
  Abstraction of a neuron. 
</figcaption>
</figure>

* `Dendrites` : 다른 뉴런들로부터 화학물질을 받는 부분으로 송신을 맡는다.
* `SOMA` (Nuclear) : 뉴런의 핵
* `Axon` : 긴 허리로 `Dendrites` 와 `Axon Terminal` 을 연결하는 부위이다. 전기적 신호를 전달한다. 
* `Axon Terminals` : 다른 뉴런들로 화학물질을 전달하는 부분으로 발신을 맡는다. 

---

### 정보 전달 방법

뉴런은 기본적으로 음전하(Negative Ion) -70mv 를 띄고 있으며, `Dendrite` 에 $Cl^{-}$ 음이온이 전달될 경우, 그대로 음전하를 띈다. 
만일 `Dendrite`에 $Na^{+}$와 같은 양이온이 전달되는 경우, 음전하를 띄던 `Dendrite` 부분은 양전하를 띄게 된다. 그리고, 전하를 일정하게 유지하는 `Neutral Charge Equilibrium`  상태를 만족하기 위해서, 양전하는 음전하 방향으로 값이 전달된다. 이 때 지나는 통로를 `Axon`이라고 부르며, `Axon Terminal` 이 양전하 상태가 되어 화학물질을 내보내는 상태로 변경된다. 

<figure style="text-align:center; display:block; grid-column:middle; " >
<img src="/assets/kor/neurons_as_neurons/neuron_state.png" style="width:120%">
<figcaption>
  Abstraction of a neuron. 
</figcaption>
</figure>


> **요약** : 뉴런이 다른 뉴런들로부터 화학물질을 받아서 양전하 상태가 되면 화학물질을 내보내고, 결과적으로 연쇄적인 상태변화가 일어난다. 


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

정보를 받는 뉴런에게는 Dendrite 가, 정보를 전달하는 뉴런에는 Axon Terminal 이 관여하며 이 두가지가 연결된 부분을 `시냅스`라고 부른다. 
따라서 시냅스는 두 개의 뉴런이 연결되어 발생하는 지점이며, 신호를 전달해주는 쪽과 받는 쪽을 구분하여 다음과 같이 부른다. 
* **(좌) Presynaptic Neuron** : `Axon Terminal`로 신호를 전달 **하는** 뉴런  $(N \rightarrow \[*\])$
* **(우) Postynaptic Neuron** : `Dendrite`으로 신호를 전달 **받는** 뉴런 $$([*] \rightarrow N)$$

<figure style="text-align:center; display:block;width:100;" >
<img src="/assets/kor/neurons_as_neurons/synapse.png" style="width:100%">
<figcaption>
  Abstraction of a neuron. 
</figcaption>
</figure>

---

하나의 뉴런이 전달하는 전기적 상태는 단순히 (+) 혹은 (-) 이지만 뉴런이 받는 화학물질은 여러 개의 뉴런들이 보내는 정보가 `Dendrites` 에 취합된다. 따라서, 여러 뉴런의 전기적 신호의 합이 최종적으로 `Axon` 을 통해서 전달하는 전기적 신호로 생각될 수 있다. 이는 각 Dendrite 들이 전달받은 음이온 또는 양이온의 신경전달물질 (Neurotransmitters) $T_1, T_2, \cdots$ 로부터 전기적 신호가 결정됨을 나타낸다. 

$$
\text{전기적 신호} = T_1 + T_2 + T_3 + T_4 + \cdots
$$

<figure style="text-align:center; display:block;width:100;" >
<img src="/assets/kor/neurons_as_neurons/transmission.png" style="width:100%">
<figcaption style="text-align:left">
  (Electricity) 뉴런의 Dendrites 에 여러 개의 뉴런이 신호를 보내고, 최합된 정보는 전기적 신호로 우측에 도달한다.  <br> (Neurotransmitters) Axon Terminal이 양전하 상태가 되면 화학물질들을 내보내고 연결되어 있는 뉴런들에 화학물질이 들어간다. 이는 각 뉴런들의 전하를 변경하는 역할을 한다. 
</figcaption>
</figure>


결과적으로 뉴런의 상태인 전기적 신호는 **무조건 1개**이다. 그리고 이 상태는 여러 개의 `Axon Terminal`에 영향을 미치므로, 전파된 정보는 여러 뉴런들에 연결되어 전파된다. 만일 전달받은 신경전달물질의 전기적 상태에 대해서 3가지 가능성이 존재한다. 
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

<figure style="text-align:center; display:block;width:100;grid-column:middle;" class='transform'>
<img src="/assets/kor/neurons_as_neurons/excitation_and_inhibition.png" style="width:100%">
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

모델 내부를 분석하는 것은 너무 어려운 일이며, 내부에서 어떤 일이 일어나는지, 파라미터들이 어떤 역활이 있는지 명확하지 않다. 
그러나 딥러닝적 뉴런에 대한 이해한다면, 모델의 내부를 뉴런들의 통신으로 생각할 수 있고 딥러닝 모델에 대해서 다음과 같은 관점이 생긴다. 

> 각 레이어마다 뉴런들이 독립적으로 입력의 활성화 정도를 계산하는 신경학적 구조 

## 뉴런과 시냅스

신경학적 뉴런에서 하나의 뉴런은 여러개의 `Dendrite` 을 가져서 정보를 받고, 출력으로 하나의 전기적 신호가 생긴다고 설명하였다. 
이 과정은 딥러닝에서 입력벡터에 대해서 파라미터를 내적을 계산하는 것과 동일한 계산이다. 

<figure style="text-align:center; display:block;width:100;grid-column:middle;">
<img src="/assets/kor/neurons_as_neurons/neuron_as_neuron.png" style="width:120%">
<figcaption>
  Abstraction of a neuron. 
</figcaption>
</figure>


신경학적 뉴런의 역할이 신호들을 결합하여, 새로운 신호를 내보내는 것으로 고려한다면 딥러닝에서 `Linear Sum` 을 하는 Weight들의 결합이 뉴런이다. 따라서, 정의하기로 Neuron 은 다음과 같다. 

> **🚀 정의: 딥러닝적 뉴런** : Weight 들의 집합 (입력에 대해서 결합을 하는 Weight) <br>
**🚀 정의: 딥러닝적 시냅스** : 개별 Weight

위에서 사람의 시냅스 개수는 600조 개수라고 하였는데 (⭐️), GPT3 의 파라미터 개수는 1750억개이다. 따라서, GPT3 가 사람보다 시냅스의 개수가 더 적을 것을 알 수 있다. 딥러닝적 뉴런의 정의를 바탕으로 실제 딥러닝 모듈들에서 몇 개의 뉴런과 시냅스가 존재하는지 살펴보자. 
대표적으로 Multi-Layer Perceptron (MLP), Convolutional Neural Network (CNN) 그리고 GPT3 모델의 뉴런들을 분석한다. 
 

> **(Optional)** Mathematical Formulation 
* 신경학적 뉴런의 연산
  * **각 뉴런들의 활성화값**: $a_1, a_2, a_3, \cdots, a_k $ (플러스 정도, 뉴런이 음전하 상태이면 0)
  * **Axon Terminal의 상태**: $w_1, w_2, w_3, \cdots, w_k $ (내보내는 신경전달물질 음~양전하)
  * **신경전달 물질을 받는 뉴런에 활성화 정도** : $\sum_k w_k \cdot a_k$
* 딥러닝적 뉴런의 연산 (`Weight` 와 `Activation` 내적)
  * **파라미터** :  $w_1, w_2, w_3, \cdots, w_k $
  * **Representation** : $a_1, a_2, a_3, \cdots, a_k $ 
  * **Output** : $\sum_k w_k \cdot a_k$

---


# 4. 신호전달 방식 비교

신경학적 뉴런과 딥러닝적 뉴런은 정보통신의 측면에서는 유사하다. 여러 가지 정보를 받고 더해서 최종적으로 아웃풋을 내보내는 구조이다. 그러나, 연산의 과정에서 몇 가지 차이가 존재한다. 


<h2 id="4-1-신호-전달-방식"> 4-1 신호 전달 방식 (Forward) </h2> 



<h2 id="4-2-피드백-방식"> 4-2 피드백 방식 (Backward) </h2> 



 Circular Signal 

<figure style="text-align:center; display:block;width:100;">
<img src="/assets/kor/neurons_as_neurons/communication.png" style="width:100%">
<figcaption>
  Abstraction of a neuron. 
</figcaption>
</figure>


뇌의 각 부분들이 역할을 나누는 것처럼 딥러닝도 뉴런들의 역할이 나뉘어져 있다. 
Classifier 는 

Branch Specialization<d-cite key="voss2021branch"/> [[link](https://distill.pub/2020/circuits/branch-specialization/)]



<figure style="text-align:center; display:block;width:100;">
<img src="https://distill.pub/2020/circuits/branch-specialization/images/Figure_1.png" style="width:70%">
<figcaption>
  © Image From branch specialization [<a href="https://distill.pub/2020/circuits/branch-specialization/" > link </a>]
</figcaption>
</figure>




# 5. 결여된 인지방식




## 6.1 물리적 몸의 상태

신체는 몸의 상태로부터 정보를 주기적으로 받으면서 반응한다. 입력이 순간적이기보다 지속적이며 반응이 연속적으로 들어오기 때문에 정보처리량이 많다. 
이와는 반대로 딥러닝 뉴런들은 입력에 대해서 반응하지만, 입력에 순간적으로 반응하는 경향이 있다. 

## 6.2 추상적 연산의 상태 

사람은 지속적으로 생각을 하며, 새로운 입력이 없더라도 뉴런이 지속적으로 활성화 된다. 
신경학적 뇌는 입력이 없더라도 추상적인 상태가 존재한다. 
그러나, 딥러닝은 입력을 처리하고 나면 끝난다. 



## 결론 