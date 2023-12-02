---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
    - name: Enver Menadjiev
      affiliations:
        name: KAIST
    - name: Youngju Joung
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: false
date: 2023-08-14
featured: true
title: '1.2 Neurons in Deep Learning'
description:  '📙 Chapter 1. Neuron <br> <em> How can we interpret neurons in deep learning?</em>'
img: "/assets/img/neurons_in_deep_learning/linear.png"
---
## Introduction 

In the previous section, we introduced neurons in deep learning by comparing neurons in neuroscience. The previous section only introduced neurons in a linear weight and bias. With the same interpretation, we introduce several neurons in deep learning modules including multi layer perceptron (MLP), convolutional neural network (CNN), recurrent neural network, and generative pretrained models (GPT). In addition, we also provide a neuron perspective on key-value memory  in GPT. We believe this interpretation of neurons can further help to understand the general computation of deep neural networks. 

## Linear Neurons in MLP

Let see the most basic module, weights in  a linear layer. The weights are a matrix whose rows are the outputs for neurons and the columns for the synapses of these neurons.  You can think that each row (neuron) computes independently the input signal $x$. 

With the assumption that $W \in \mathbb{R}^{\textcolor{red}{m} \times \textcolor{blue}{n}}$
* 🌱 number of neurons : $\textcolor{red}{m}$
* 🌱 number of synapses per a neuron : $\textcolor{blue}{n}$
* 🌱 number of synapses in total : $\textcolor{red}{m} \times \textcolor{blue}{n}$

MLP is a series of linear neurons with activations in between them. The activations are thresholds to pass the information with valid signals. For example, ReLU cuts negative signals and Tanh provides gating mechanisms of signals. Therefore, in the interpretation of neurons, we can think of activations as modifiers of neurons’s outputs. 

The Figure below shows two layer MLP. Each layer has two neurons and the neurons in the first layer have three synapses for each neuron, while the neurons in the second layer have two synapses for each neuron. 

<figure style="text-align:left; display:block;width:100;" >
<img src="/assets/img/neurons_in_deep_learning/linear.png" style="width:100%">
<figcaption>
  There are 2 neurons in each layer and 4 neurons in total. Synapses in a neuron computes dot-product with the input signals. 
</figcaption>
</figure>

The Figure below shows the four layer MLP. In each layer, three neurons exist and each neuron has three synapses. Note that all the pre-neurons and post-neurons are connected with synapses. We think that is why MLP is called a densely connected neural network. 

<figure style="text-align:left; display:block;width:100;">
<img src="/assets/img/neurons_in_deep_learning/mlp.png" style="width:100%">
<figcaption>
  An example of four layer MLP. The first panel shows weight perspectives, while the second panel shows neuron’s perspective. Note that all the neurons output only a single scalar respectively.   
</figcaption>
</figure>

## Convolutional Neuron 

Convolutional layer is also considered as a linear layer as we can reformulate the convolution operation as a linear operation. However, we use another tool to interpret the convolutional neuron. The most frequent way of creating a convolutional layer is  using the notation: (`out_channels`, `in_channels`, `kernel`, `kernel`, ..). With this notation, the `out_channels` parts are the same with the rows in the linear neuron and (`in_channels`, `kernel`, `kernel`) are the synapses for a neuron. With this interpretation, the only difference with the linear layer is (`kernel`, `kernel`) part where the convolution operation is applied in the 2D plane. If you assume that kernel size is 1, then you can think of a linear neuron! 

* 🌱 number of neurons :  `out_channels`
* 🌱 number of synapses per a neuron :  `in_channels`$\times$ `kernel` $\times$ `kernel`
* 🌱 number of synapses in total :  `out_channels` $\times$  `in_channels` $ \times$ `kernel` $\times$ `kernel`


---

## Recurrent Neuron

Beside MLP and CNN, the other basic module is a recurrent neural network (RNN) which successively computes an input with linear operations in it with hidden states. Computationally, the recurrent neuron is the same with the linear neuron except input and output types. Note that the recurrent neuron should provide outputs which are re-injected to the input of that neuron. The advanced RNN modules such as LSTM <d-cite key="hochreiter1997long"/> and GRU <d-cite key="chung2014empirical"/> use the gating mechanism in them to process the long-term memory well. These architectures do not change the interpretation of neurons as they are combinations of linear neurons.


---

## Neurons in GPT Block 

Now we discuss neurons in more advanced architectures such as GPT. GPT consists of a token embedding module, transformer decoder blocks and task specific heads such as LMHead. We verify neurons in a GPT block and leave the interpretation of other modules as they are just linear-like neurons. GPT block has two main parts MLP and self-attention (or cross-attention), and layer-norm. 

#### MLP 

MLP block is a two layer MLP whose first layer is sometimes called key and second layer sometimes is called value. We use $d_{model}$ which is the dimension of GPT representations to describe the number of neurons and synapses. The key part is a linear layer with weight $$W_{key} \in \mathbb{R}^{\textcolor{red}{4\cdot d_{model}} \times \textcolor{blue}{d_{model}}}$$ whose number of neurons is four times larger than the number of synapses. On the other hand the value part is a linear layer with weight $$W_{value} \in \mathbb{R}^{\textcolor{red}{ d_{model}} \times \textcolor{blue}{4\cdot d_{model}}}$$ whose number of neurons is four times smaller than the number of synapses. Therefore, this architecture is a bottleneck architecture and many researchers interpret it as a neural memory. 

Consider $d_{model} = 12,288$ which is the hidden dimension size of GPT3 (175B).

* 🌱 number of neurons :  49,152 (key neuron) + 12,288 (value neuron)
* 🌱 number of synapses per a neuron : 12,288 (key neuron), 49,152 (value neuron)
* 🌱 number of synapses in total :  **1,207,959,552**  <br> (= 49,152 $\times$ 12,288 (key neuron) +  12,288 $\times$ 49,152 (value neuron))



#### ATTN

In ATTN, there are four linear layers: query, key, value, output and each layer has weight $$W \in \mathbb{R}^{\textcolor{green}{d_{model}} \times \textcolor{green}{d_{model}}}$$. Therefore, we can easily interpret neurons in ATTN<d-footnote> If you assume QK, OV  circuits, the number of neurons are reduce by 2 times. <d-cite key="elhage2021mathematical"/> </d-footnote>. Note that self-attention and cross-attention are about how to use these neurons and do not affect the interpretation of neurons

Consider $d_{model} = 12,288$ which is the hidden dimension size of GPT3 (175B).

* 🌱 number of neurons :**61,440** = 12,288 $\times$ 4 (Q, K, V, O)
* 🌱 number of synapses per a neuron : : **12,288** (Q, K, V, O)
* 🌱 number of synapses in total :  **603,979,776** = (12,288 $\times$ 4) $\times$ 12,288

---

## Neurons in GPT3 (175B)

Finally, we count the number of neurons and synapses in GPT3 (175B) blocks with the above interpretation<d-footnote> Caveats: We do not count the layer-norm in a block and the number of weights would not exactly match with the number of synapses in this case. Also, bias is a parameter in GPT, but is not counted for neuron and synapse. </d-footnote>.   GPT3 has 96 blocks and each block has MLP whose number synapses is **1,207,959,552** and ATTN whose number of synapses is **603,979,776**. We can simply add these two and multiply 96 to compute the total number of synapses.


* 🍀 (`GPT3`) number of synapses : **173B**  <br> (173,946,175,488 = 96 $\times$ (1,207,959,552 + 603,979,776)) <d-footnote>  2B difference (175B and 173B) is due to bias, embedding, layer-norm and LM heads modules. </d-footnote>
* ☘️ (`GPT3`) number of neurons :  **11M** <br> (11,796,480 = 96 $\times$(61,440+61,440))
* ☘️ (`GPT3`) number of synapses per a neuron : **14,745** 

As humans have 7,000 synapses per neuron, we can think that GPT3 has a similar number or even larger number of connections between neurons. However, humans have much more neurons compared to GPT.  We believe such a difference can affect the general learning process. For example, if the number of neurons is large, we can see them as an ensemble of simple knowledge. On the other hand, if the number of synapses is large, expert neurons will provide knowledge.


## Conclusion

As we think more about a deep neural network with a metaphor to a human brain, the computational graph is highly interpretable and the individual components handle very simple computations like neurons in our brain. Recent work in interpretability is about discovering meanings of neurons and the work like this can shed light on the interpretation of the deep neural network. We believe still deep dissection of neural network can give better interpretation of deep neural networks. 


--- 

## Differences between Neuroscience and Deep Learning

### Rate-coding

Biological neurons communicate via receiving and transmitting excitatory spikes. Having received multiple spikes, neuron is more likely to produce multiple spikes of its own. However, unlike in deep learning, biological neurons do not receive spikes simultaneously. Moreover, neuron rates may coincide, and synchronize between multiple neurons, which is not presented in the deep learning.

<figure style="text-align:left; display:block;width:100;">
<div style="display: flex; justify-content: center">
<img src="/assets/img/neurons_in_deep_learning/rate_coding.png" style="width:75%; margin: auto 0;">
</div>
<figcaption>
  The key-neurons output scalars which are multiplied with weights (synapses) in the value-neurons. Then, the values neurons compute them (linear sum) to make a single output respectively.
</figcaption>
</figure>

### Synaptic strength

Spikes are not equal, since the after reaching axon terminal the amount of electrical energy strongly depends on the strength of the synapse. Therefore, with the same output signals from preceding neurons, the received signal may differ. Similarly, the strength of the signal in deep learning is usually presented by the weight of synapse.

<figure style="text-align:left; display:block;width:100;">
<img src="/assets/img/neurons_in_deep_learning/synaptic_strength.jpg" style="width:100%">
<figcaption>
  The key-neurons output scalars which are multiplied with weights (synapses) in the value-neurons. Then, the values neurons compute them (linear sum) to make a single output respectively.
</figcaption>
</figure>


### Excitatory and inhibitory neurotransmitters

If biological neurons are examined closely, it becomes evident that there are two main classes of neurotransmitters <d-footnote>Elements, transmitted with signal </d-footnote>, namely excitatory, and inhibitory. The former ones are responsible for amplifying transmitted signal, while the latter decrease its strength. Although most of the neurons are excitatory, inhibitory neurons play crucial role in selecting, and routing information, preventing epileptic activity <d-footnote> Chaotic firing of many neurons in the network</d-footnote>.

### Conclusion

* The deep learning neuron receives inputs, or activations, from other neurons. The activations are rate-coded representations of the spiking of biological neurons.
* The activations are multiplied by synaptic weights. These weights are models of synaptic strengths in biological neurons, and also model inhibitory transmission, in that the weights may take on negative values.
* The weighted activations are summed together, modeling the accumulation process that happens in the cell body of a biological neuron.
* A bias term is added to the sum, modeling the general sensitivity of neuron.
* Finally, the summed value is shaped by an activation function — typically one that limits the minimum or maximum output value (or both), such as a sigmoid function. This models the intrinsic minimum spiking rate of biological neurons (zero) or the maximum rate (due to details in the physiological mechanisms by which spikes are generated).

## Steps to match Biological and Deep Learning Neurons

### Temporal Coding

Deep learning relies on rate-based coding, in which each neuron’s activation is a single numeric value that models the average spiking rate in response to a given stimulus (be it from other neurons or from an external stimulus). The collective set of spiking rate values within a single layer of the network is typically organized as a vector of numbers, and this vector is referred to as the representation of an external stimulus, at that layer.

The expressiveness of rate-based neural coding is much lower than that which is possible with neural codes (representations) based on the relative time between spikes on multiple neurons. As a simple example of the existence of this type of code in biology, consider the auditory system. When sound waves reach our ears, our brain processes them to determine the type of animal, object, or phenomenon that produced the sound, but also to estimate the direction from which the sound came (localization). One way in which sound location is determined is based on the fact that sounds from the right will reach the right ear first, then the left ear. Auditory neurons close to the right and left ears exhibit spike timing that reflects this acoustic timing difference. Auditory neurons the are more medially located (near the midline of the body) receive input from neurons near both ears and are selective for the location of the sound, thanks to this temporal coding.

<figure style="text-align:left; display:block;width:100;">
<img src="/assets/img/neurons_in_deep_learning/ear_processing.png" style="width:100%">
<figcaption>
  The key-neurons output scalars which are multiplied with weights (synapses) in the value-neurons. Then, the values neurons compute them (linear sum) to make a single output respectively.
</figcaption>
</figure>

More generically, consider the simple example of a single neuron receiving input from two other neurons, each of which send identical input: a short train of N uniformly spaced (in time) excitatory spikes over 100 ms. All else being equal, this will generate some stereotypical response in the receiving neuron. In contrast, if one of the input neurons sent all if its spikes in the first 20 ms (of the 100 ms interval), and the other input neuron sent all of its spikes in the final 20 ms, the response of the receiving neuron is likely to be notably different. Thus, even though the spiking-rate of the input neurons is identical in each scenario (10N spikes/sec) the temporal coding is quite different, and the response of the receiving neuron can be quite different as well. Importantly, there can exist many input-output combinations when using a temporal code, even when the number of input spikes is low, constant, or both. This is what we mean by a more expressive coding scheme. In regards to AI, a model that utilizes temporal coding can conceivably perform much more complex tasks than a deep learning model with the same number of neurons.

<figure style="text-align:left; display:block;width:100;">
<div style="display: flex; justify-content: center">
<img src="/assets/img/neurons_in_deep_learning/temporal_coding.png" style="width:75%">
</div>
<figcaption>
  The key-neurons output scalars which are multiplied with weights (synapses) in the value-neurons. Then, the values neurons compute them (linear sum) to make a single output respectively.
</figcaption>
</figure>

### Inhibitory neurons

Based on our simple description of biological and deep learning neurons, the distinction between excitatory and inhibitory neurons can be mimicked by deep learning neurons. Namely, one can mimic a biological inhibitory neuron simply by ensuring its deep learning equivalent has negative values for all synaptic weights between its axons and the dendrites of neurons to which it projects. Conversely, when mimicking a biological excitatory neuron, such synapses should always have positive weights. However, training and implementation will be easier if one simply requires that all synapses are positive-valued (perhaps by applying a ReLU function to the weights after each training iteration), and use an activation function that produces negative (positive) values for the inhibitory (excitatory) neurons.

It’s not certain, but one possibility is that the use of explicit inhibitory neurons helps to constrain the overall parameter space while allowing for the evolution or development of sub-network structures that promote fast learning. In biological systems, it is not necessary for the brain to be able to learn any input-output relationship, or execute any possible spiking sequence. We live in a world with fixed physical laws, with species whose individual members share many common within-species behavioral traits that need not be explicitly learned. Limiting the possible circuit connectivity and dynamic activity of a network is tantamount to limiting the solution space over which a training method must search. 

Another potential benefit of inhibitory neurons, related to the use of structured canonical circuits as just mentioned, is that inhibitory neurons may effectively "gate off" large numbers of neurons that are unnecessary for processing of a given sample or task, thereby lowering energy requirements.

### Low-energy spike-based hardware

The energy efficiency of biological neurons, relative to conventional computing hardware, is largely due to two characteristics of these neurons. Firstly, biological neurons transmit only short bursts of analog energy (spikes) rather than maintaining many bits that represent a single floating-point or integer number. In conventional hardware, these bits require persistent energy flow to maintain the 0 or 1 state, unless much slower types of memory are used (non-volatile RAM).

Secondly, memory is co-located with processing in biological neurons. That is, the synaptic strengths are the long-term memory of the network (recurrent connectivity can maintain short-term memory, but that’s a topic for some other post), they are involved in the processing (spike weighting and transmission), and are very close to other aspects of the processing (energy accumulation in the cell body). In contrast, conventional hardware is regularly transmitting bits from RAM to the processor — a considerable distance and a considerable energy drain.

---


## (Appendix) Interpret Key Neurons and Value Neurons 

In the MLP section, we discuss the key-value neurons. In this section, we provide more detailed neuron-perspectives on these modules. The Figure below shows key-value computation with sizes, 3 for input, 8 for key, and 2 for value neurons <d-footnote> We use 3 for the better visualization of the input, which is originally 2 in GPT.  </d-footnote>. 

<figure style="text-align:left; display:block;width:100;">
<img src="/assets/img/neurons_in_deep_learning/key-value.png" style="width:100%">
<figcaption>
  The key-neurons output scalars which are multiplied with weights (synapses) in the value-neurons. Then, the values neurons compute them (linear sum) to make a single output respectively.
</figcaption>
</figure>


If we just assume that a neuron just provide a single output, we can interpret a neuron in a row-wise manner. That is, we don't think about the meaning of weights and just consider the output value of each neuron. The Figure below shows the neuron-wise interpretation. 


<figure style="text-align:left;">
<img src="/assets/img/neurons_in_deep_learning/neuron_inter.png" style="width:100%">
<figcaption>
  The row-wise neuron interpretation. As a single neuron has multiple synapses, the aggregated signal will go out and further be cut with activations. 
</figcaption>
</figure>


Unlike the neuron-wise interpretation where we interpret each neuron separately, we usually interpret multiple neurons together by considering the directional vector. In this case, the weights in value-neurons are not just used for aggregation of synapses, but could be interpreted by directionally meaningful information. The Figure below shows the example. In this case, the key is used to scale the vectors. 

<figure style="text-align:left;">
<img src="/assets/img/neurons_in_deep_learning/value_inter.png" style="width:100%">
<figcaption>
  Column-interpretation of neurons. It is more natural to think a weight matrix as directional vectors. The key-neurons output scales for the directional vectors. 
</figcaption>
</figure>