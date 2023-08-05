---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-08-4
featured: true
title: 'Neuron in Neuroscience and Deep Learning'
description: ''
---

## Introduction 

There are several attempts to understand human in the fields of biology, neuroscience, ethology, and philosophy. Especially, neuroscience interprets human with neurons in a brain. Neurons in a brain process massive information and the intelligence increases as the number of neurons increases. However, understanding the general role of a single neuron is hardly discovered as we can not test the neuron separately excluding other neurons. However, unlike the neurons in neuroscience, neurons in deep learning could be tested easily as we can debug the network. Recent studies have shown the role of individual neurons in both vision and NLP domains and we can understand the interaction between neurons well. If the neurons in our brain compute and  process the signals with the neurons in deep learning, we can further understand neurons in both field. To do this, we should know how similar neurons in neuroscience and deep learning are. 

The reason why I wrote this post, even though I studied deep learning, is that understanding the neurons in neuroscience can further helps to reveal the mechanisms of neurons in deep learning. To components are similar to each other, but have distinct difference such as learning processes. We study the question **How much neurons are similar in both fields?** Before answering this question we should answer this question. **What is neurons in deep learning?**

## Neuroscience

Human body has cell and there are cells called **neuron** whose role is the transmission of signals.  Neurons communicate each other by chemical materials called 
**synapse** which is the connection part between two neurons. In human brain, there are 860B number of neurons and a single neuron has 7,000 synapses. Therefore, human brain has 6 trillion synapses. 

$$
\begin{gather}
\text{Number of Neurons} \times \text{Number of Synapses per a neuron} \approx \text{Total number of synapses} \\ 
\Rightarrow 860 \text{B} \times 7,000 \approx 6 \text{trillion}
\end{gather}
$$

The number of synapses is the number of connections and the neurons has information in it. When a single neuron is fired, we can consider the successively fired neurons like a chain. Consider the following example.  

1. Eyes get light signals (**Dog image** 🦮)  
2. neurons in eyes are fired and signal is transmitted to the neurons in the brain (**Neurons related to vision** 👀)
3. Several neurons in a brain are affected by the vision neurons
  * Emergence of vocabulary "dog" (**Dog Neuron** 🐶)
  * Emergence of vocabulary "cat" by "dog" (**Cat Neuron** 😸)
  * Prediction of the dog movement (**Future Prediction Neuron** ⌛️)
  * Emergence of traumas related to a dog (**Long-term Memory Neuron** 🐾)
  * Trauma makes me feel bad (**Emotion Neuron** 😢)

Note that even though we only observed the dog image, other neurons related to the **dog**  are fired. Understanding the circuits of neurons are like finding such consequent events of neurons. 

---

### Neuron

> These videos can help your understanding of neurons
<div>
<iframe width="340" height="315"
src="https://www.youtube.com/embed/SczOfOXY17U">
</iframe>
<iframe width="340" height="315"
src="https://www.youtube.com/embed/GIGqp6_PG6k">
</iframe>
</div>

#### Basic Structure

Neurons communicate with each other with chemical materials such as Na+ and Cl-, and process the signal in terms of electricity. To understand this communication process, lets see what components a neuron has. 


<figure style="text-align:center; display:block;" >
<img src="/assets/kor/neurons_as_neurons/neuron.png" style="width:100%">
<figcaption>
Figure. Basic structure of a neuron. The dendrite receives input chemical, axon transfers the electricity, and axon terminal outputs chemical material again. 
</figcaption>
</figure>

* `Dendrites` : Obtains chemical materials from other neurons (receiver)
* `SOMA` (Nuclear) : The nuclear of a neuron
* `Axon` : connects `Dendrites` and `Axon Terminal` and transfers the electrical signal.
* `Axon Terminals` : transmits chemical materials to other neurons (publisher)

#### Communication 

A neuron is normally a negative state (-70mv) and if a `Dendrite` receives negative ion, the state is still negative. On the other hand, if `Dendrite` receives positive ions such as $Na^{+}$, the dendrite part becomes positive state. Then, the electricity flows from `Dendrite` to `Axon Terminal` in a neuron to make `Neutral Charge Equilibrium` state. The pipe for the transmission is called `Axon` and the leaf part is `Axon Termnial`. When the `Axon Terminal` becomes positive state, neurotransmitters are spread. 

<figure style="text-align:center; display:block; grid-column:middle;">
<img src="/assets/kor/neurons_as_neurons/neuron_state.png" style="width:180%">
<figcaption>
  How a neuron transmits electrical signal from dendrites to axon terminals. Neuron is basically negative state (-), the activated part are relatively positive state (+). 
</figcaption>
</figure>

In summary, a successive propagation of chemical makes the communication. 

### Synapse

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

#### Aggregation of Received signal

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


사람은 수많은 인지과정을 거치므로 정확히 어떤 뉴런이 어떤 역할을 가지는지, 현재 뇌는 어떤 정보를 처리하고 있는지 명확하지 않다. 또한, 뉴런들의 값을 모두 추적하기 위해서도 많은 현대기술들이 필요하다. 그러나 이와 다르게 딥러닝 모델들은 입력에 대해서 딥러닝 모델의 뉴런들이 어떤식으로 반응하는지 값을 추적할 수 있으며, 반응을 확인할 수 있다. 또한 뉴런들간의 연결이 존재한다면 이를 정량적으로 디버깅할 수 있다. 만일 딥러닝의 뉴런이 신경세포의 통신과정과 비슷하게 정보를 저장하고 전달한다면, 딥러닝의 뉴런을 분석하는 것이 신경학의 뉴런을 분석하는 열쇠가 될 수 있다. 그런데, *딥러닝에서 뉴런이라는 것은 정확하게 뭘까?* 이에 대한 답을 내려보고자 한다. 

---

## Deep Learning 

딥러닝 모델의 내부에 대해서 가지고 있는 생각은 대부분 다음과 같을 것이다.  

> 수많은 파라미터들이 레어이마다 존재하며, 입력과 출력을 End-to-End 로 계산하는 블랙박스 모델 

모델 내부를 분석하는 것은 너무 어려운 일이며, 내부에서 어떤 일이 일어나는지, 파라미터들이 어떤 역할이 있는지 명확하지 않다. 
그러나 딥러닝적 뉴런에 대한 이해한다면, 모델의 내부를 뉴런들의 통신으로 생각할 수 있고 딥러닝 모델에 대해서 다음과 같은 관점이 생긴다. 

> 각 레이어마다 뉴런들이 독립적으로 입력의 활성화 정도를 계산하는 신경학적 구조 

### Neuron & Synapse

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


## Comparison 


### Forward 


### Backward

## Type of Information

The most significant unfairness of deep learning compared to humans is the difference in input. Human  eyes receive information as input every moment, and about 200 images are received in high resolution per second. But the deep learning models take an input of the level of an image.
Therefore the recent models are designed to take more inputs in order to approximate the amount of information a human receives. However, deep learning still has the disadvantage of reacting only when input is given.
→ What can deep learning do if there is no input?


### Absence of State

In humans, neurons can respond and process information without being given input. 
(e.g., you can recall a picture in your mind and draw conclusions or create new ideas from that picture)
This process is considered to retrieve information which is stored in the brain's long-term memory, but the deep learning parameters are configured to perform operations on given inputs and there is no memory.


### State of Creature

Another difference is the type of input. While humans receive information through sensory organs, deep learning models have a fixed input space, and visual, linguistic, or audio information comes in through the input layer. Therefore, the information processed by deep learning is only the input and the output calculated by the internal neurons. However, for humans, there is additional information in addition to the information received as input. It is the state of the body or cognition.


### State of body

> Do deep learning models change calculation results just because they are tired? No.


In humans, even given the same input, the result can be changed by current hormonal conditions. For example, If you feel sad, you can have different interpretations of the same picture. This suggests that the processing of neurons can be different depending on the body's states.


### State of cognition

> Does deep learning keep thinking about information it has seen before? No.

Cognitive contrast is a principle in which the next information is distorted and interpreted by the previously seen information. For example, a tiger next to an elephant is perceived as small because the elephant is large.
However we cannot be sure that deep learning neurons analyze things based on this principle of cognitive contrast. They are more likely to interpret simply depending on the information currently available.

## Conclusion 