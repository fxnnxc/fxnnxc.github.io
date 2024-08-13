---
layout: distill
authors:  
    - name: Bumjin Park
      affiliations:
        name: KAIST
    - name: Youngju Joung
      affiliations:
        name: KAIST
    - name: Enver Menadjiev
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-08-04
featured: true
title: 'Neuron in Neuroscience and Deep Learning'
description: 'How can we compare neurons in neuroscience and deep learning? <br> [📚 Neuron Series 1]'
img: /assets/kor/neurons_as_neurons/communication.png
tags: neuron
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

* `dendrites` : Obtains chemical materials from other neurons (receiver)
* `SOMA` (Nuclear) : The nuclear of a neuron
* `Axon` : connects `dendrites` and `axon terminal` and transfers the electrical signal.
* `Axon terminals` : transmits chemical materials to other neurons (publisher)

#### Communication 

A neuron is normally a negative state (-70mv) and if a `dendrite` receives negative ion, the state is still negative. On the other hand, if `dendrite` receives positive ions such as $Na^{+}$, the dendrite part becomes positive state. Then, the electricity flows from `dendrite` to `axon terminal` in a neuron to make `Neutral Charge Equilibrium` state. The pipe for the transmission is called axon and the leaf part is `axon terminal`. When the `axon terminal` becomes positive state, neurotransmitters spread. 

<figure style="text-align:center; display:block; grid-column:middle;">
<img src="/assets/kor/neurons_as_neurons/neuron_state.png" style="width:180%">
<figcaption>
  How a neuron transmits electrical signal from dendrites to axon terminals. Neuron is basically negative state (-), the activated part are relatively positive state (+). 
</figcaption>
</figure>

In summary, a successive propagation of chemical makes the communication. 

### Synapse

Synapse is the connected parts of `dendrite` and `axon terminal`. Synapse is the location where the communication between two neurons occur, and we call the sender and receiver as follows:
`N: Neuron, [*] Other Neurons` 

* **(Left) Presynaptic Neuron** : a neuron transmits signal with `axon terminal`  $(N \rightarrow \[*\])$
* **(Right) Postynaptic Neuron** : a neuron receives with `dendrite` $$([*] \rightarrow N)$$

<figure style="text-align:center; display:block;width:100;" >
<img src="/assets/kor/neurons_as_neurons/synapse.png" style="width:100%">
<figcaption>
  Figure. Two neurons are connected and  the synapse is the connection. The sender gives signal with the axon terminal and the receiver gets signal wit the dendrite. 
</figcaption>
</figure>

#### Aggregation of Received signal


The electrical signal flows in a neuron is either (+) or (-), but the received chemicals at dendrites are combined. Therefore, the aggregated (summed) electrical signal is transferred along `Axon`. In other words, the ions $T_1, T_2, \cdots$  at dendrites are summed and generate the electrical signal.  
$$
\text{Electrical Signal} = T_1 + T_2 + T_3 + T_4 + \cdots
$$

<figure style="text-align:center; display:block;width:100;" >
<img src="/assets/kor/neurons_as_neurons/transmission.png" style="width:100%">
<figcaption style="text-align:left">
  Figure. (Up) Several neurons transmit chemicals to the postsynaptic neuron. (Down) as axon becomes positive state, axon terminals spread chemicals to the connected neurons. 
</figcaption>
</figure>


Recall that there is only one electrical signal, but the several axon terminals transmit different kinds of neurotransmitters. 
Neurotransmitters are categorized by whether the chemical further excites the connected neurons. The excitatory neuron spreads positive ions, while the inhibitory neuron spreads negative ions. See **Thread: Circuits** <d-cite key="cammarata2020thread"/> [[post](https://distill.pub/2020/circuits/)] for the detailed examples in CNN. 

> **Excitatory and Inhibitory Neurotransmitters**
* 🔥  Excitatory Neurotransmitters
  * Na+ 
  * Acetylcholine
  * Noradrenaline 
  * Glutamate 
* ❄️ Inhibitory Neurotransmitters
  * Glycine
  * GABA (gamma-aminobutyric acid)
  * Cl-

<figure style="text-align:center; display:block;width:100;grid-column:middle;">
<img src="/assets/kor/neurons_as_neurons/excitation_and_inhibition.png" style="width:120%">
<figcaption>
  Excitatory neuron make the next neuron activated, while the inhibitory neuron prevents the activation of the connected neuron. 
</figcaption>
</figure>


---

## Deep Learning 

### Neuron & Synapse

In the neuroscience section, we introduced the electrical signal made by the multiple chemical signals from dendrites. 
This operation is same with the dot product which results in a single scalar. 


<figure style="text-align:center;">
<img src="/assets/kor/neurons_as_neurons/neuron_as_neuron.png" style="width:100%">
<figcaption>
  Figure. The similarity between a neuron in neuroscience and a neuron in deep learning. 
</figcaption>
</figure>

In linear layer, we can interpret each row as a neuron

#### 🚀 Neuron in deep-learning motivated by the neuroscience.** 
**Neuron** : collection of weights which outputs a single scalar. 
**Synapse** : each weight in the neuron

* Neuron in neuroscience
  * (In) **the amount of $K$ chemicals**: $a_1, a_2, a_3, \cdots, a_k $ 
  * (Weight) **K dendrites**: $w_1, w_2, w_3, \cdots, w_k $ (positive or negative)
  * (Out) **How much activation occurs** : $\sum_k w_k \cdot a_k$

* Neuron in deep learning 
  * (In) **Neuron Input** :  $a_1, a_2, a_3, \cdots, a_k $
  * (Weight) **Neuron Interpretation** : $w_1, w_2, w_3, \cdots, w_k $ 
  * (Out) **Neuron Output** : $\sum_k w_k \cdot a_k$

In the linear layer, we have weights and biases, $W\in \mathbb{R}^{N\times K}$ and $\mathbf{b}\in \mathbb{R}^{N}$. When we interpret this (1) there are `N` neurons, (2) each neuron accumulate $K$ signals, and (3) output is linear combined.  As all $N$ neurons output a single scalar, the dimension of output is $N$.

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

* ⭐️ Number of Neurons : $N$ (Dimension of Output)
* ⭐️ Number of Synapses : $N\times K$ (Number of Weights)


## Comparison 

In the previous section, we found that the dot-product could be interpreted as the computation of a neuroscience neuron. In this section, we compare how communications of two neurons are similar. In deep learning, we consider two message passing paths, `forward` and `backward`.  However, the neurons in neuroscience only have `forward` pass to communicate with each other. In addition, the learning paradigm is different that the deep learning neuron learns from the back-propagation of signals, which is not true in neuroscience! 


### Forward 

The figure below shows the major difference in communication. Note that the neurons in neuroscience can form a circular message passing as they are located in the 3D physical system, while the deep learning has only a single forward pass and each neuron process information only one time <d-footnote> If we apply the parameter sharing, a single neuron can process information multiple times. </d-footnote>. Note that even RNN which is known to process multiple inputs also computes information only one time. 

In addition, neurons in neuroscience communicate in asynchronous manner, but the computation of neurons in deep learning is synchronous for every layer. Therefore, humans are more stochastic than neurons in deep learning. 


<figure style="text-align:center; display:block;width:100;">
<img src="/assets/kor/neurons_as_neurons/communication.png" style="width:100%">
<figcaption>
  Figure. How neurons communicate with each other. For the neurons in the neuroscience, they could form a circular communication, while the deep learning neurons from only one-directional signal processing.  
</figcaption>
</figure>

### Backward

The backward is a natural way of training a deep neural network. The weights in neurons are updated to minimize an objective and back-propagation signal passes in backward. Unlike this, neurons in neuroscience can receive and send information with only two fixed components, dendrites and axon terminals. Therefore, we can not think the neurons in neuroscience use back-propagation to learn.  Recent work of Geoffrey Hinton [[scholar](https://scholar.google.com/citations?user=JicYPdAAAAAJ&hl=ko&oi=ao)] is inspired from this observation, and he proposed forward-forward algorithm <d-cite key="hinton2022forward"/>.


## Type of Information

The most significant unfairness of deep learning compared to humans is the difference in input. Human  eyes receive information as input every moment, and about 200 images are received in high resolution per second. But the deep learning models take an input of the level of an image. Although recent models are designed to take more inputs in order to approximate the amount of information a human receives, deep learning still has the disadvantage of reacting only when input is given. 

In addition, unlike humans, deep learnings only work when an input is given: *What can deep learning do if there is no input?*


### Absence of State

In humans, neurons can respond and process information without being given input
(e.g., you can recall a picture in your mind and draw conclusions or create new ideas from that picture).
This process is considered to retrieve information which is stored in the brain's long-term memory, but the deep learning parameters are configured to perform operations on given inputs and there is no memory.


### State of Creature

Another difference is the type of input. While humans receive information through sensory organs, deep learning models have a fixed input space, and visual, linguistic, or audio information comes in through the input layer. Therefore, the information processed by deep learning is only the input and the output calculated by the internal neurons. However, for humans, there is additional information in addition to the information received as input. It is the state of the body or cognition.


### State of body

> Do deep learning models change calculation results just because they are tired? No.

In humans, even given the same input, the result can be changed by current hormonal conditions. For example, If you feel sad, you can have different interpretations of the same picture. This suggests that the processing of neurons can be different depending on the body's states.


### State of cognition

> Does deep learning keep thinking about information it has seen before? No.

Cognitive contrast is a principle in which the next information is distorted and interpreted by the previously seen information. For example, a tiger next to an elephant is perceived as small because the elephant is large. However we cannot be sure that deep learning neurons analyze things based on this principle of cognitive contrast. They are more likely to interpret simply depending on the information currently available.
