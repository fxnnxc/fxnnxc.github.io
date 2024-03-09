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
disqus_comments: false
date: 2023-08-22
featured: true
title: '1.3 Interpret Neurons with Neurons'
description:  '📙 Chapter 1. Neuron <br> <em> Can you find meaningful neurons with neurons?</em>'
img: "/assets/img/neurons_interpret_neurons/interpretation_neuron.png"
---


## Interpret Neurons with Neurons


Inside of a deep neural network, numerous neurons exist, and each of them activated by some specific patterns. We can easily assume the existence of such neurons in the complex inside, but finding such neurons requires expensive efforts such as Network Dissection and  CLIP Dissection. Unlike visualizing the detected region of a neuron, a much easier way is learning a classifier to verify the semantic meaning of neurons so that the weight of the classifier reveals the relationship between the activation of the neuron and a specific pattern. 
To further understand the easiness of such a method, assume the situation where we want to find a neuron detecting “The Stripe Pattern in Zebra”.

* **Difficulty 1**: there are numerous neurons in a network which are fired with a zebra image. <br> (Multiple neurons are activated by a single image)  
* **Difficulty 2**: we don’t know how much to be activated for the pattern.  <br> (The proper threshold for activation)
* **Difficulty 3**:  The proportion of activation might not be 100%. In what threshold, can we Difficulty that the neuron has the semantic pattern? <br> (How much `# Activated Samples` / `# Total Samples?` )

These difficulties has the same goal: 
<blockquote style='background-color:#DDFFFF' markdown="1">
 How can we determine the features of a neuron with activation patterns?
</blockquote>

To solve this problem with another deep neural network, we must consider two types of neurons, a neuron capturing a semantic pattern and a neuron interpreting the activation of the neuron. Two neurons have the following domain and co-domain.
* Pattern neuron : Input → Activation Score ($x \mapsto h$)
* Interpretation neuron :  Activation Score → Concept ($h \mapsto C$) 

## Pattern Neuron 

A pattern neuron has positively correlated activation patterns with respect to a specific semantic image, such as strip patterns. As the intensity of the activation is stronger, the activation of the neuron is higher. On the other hand, if the pattern does not exist in the input, the neuron must not propagate signals.  In addition, this assumption could be stochastic as the activation patterns could have noise. 


<figure style="text-align:left; display:block;width:100;" >
<img src="/assets/img/neurons_interpret_neurons/feature_neuron.png" style="width:100%">
<figcaption markdown="1">
Figure. Several inputs are mapped to the neuron's output. They leave activation signals.  With the assumption that the neuron has semantic meaning, large value of `F` will be considered to have more degree of a feature. 
</figcaption>
</figure>


The mathematical formulation of pattern neuron is as follows:




<blockquote style='background-color:#FFEFEF; font-style:normal;' markdown="1" >
**Formulation for a pattern neuron $$h = f_n(x)$$** 
* $x$ : an input 
* $h$ : activation of the neuron
* $f_n$ : a function maps input $x$ to activation $h$.
* $c_n$ : the concept related to neuron $n$ 
</blockquote >

## Interpretation Neurons

The interpretation neuron takes activation score as an input and outputs the decision of concept with a separate neuron. Note that the interpretation neuron is not internal neurons of the original network. They are separate neurons which will be trained for the interpretation of activation values. 


<figure style="text-align:left; display:block;width:100;" >
<img src="/assets/img/neurons_interpret_neurons/interpretation_neuron.png" style="width:100%">
<figcaption>
Figure. Several inputs are mapped to the neuron and leave activations. The interpretation neuron will be trained to classify the concept patterns with the activation signals. 
</figcaption>
</figure>

The interpretation neuron is a function $g$ which can have the most simplest form as follows:

$$
g_n = \sigma (wh+b) 
$$

Here $\sigma$ is a sigmoid function to be used for the decision of a concept. With such formulation we can interpret the decision as follows:
If there is positive correlation between $h$ and concept $c$, the weight will be positive 
If there is no correlation between the activation and concept, the weight will be close to zero. 


<blockquote style='background-color:#FFEFEF; font-style:normal;' markdown="1" >
**Formulation of an interpretation neuron: $$c = g(h)$$**
* $h$ : an input 
* $c$ : the concept to be verified for an input
* $g$ : a mapping from $h$ to $c$. 
* $w,b$ : weight and bias for the classifier $g$. 
</blockquote >

This classifier can help us to mitigate the difficulties 2 and 3 (The proper threshold for activation, and How much # Activated Samples / # Total Samples?). However, difficulty 1 (numerous neurons) is not solved with learning a classifier with input size 1. To solve it, we further formulate a classifier with high-dimensional input space.  





## Interpretation Neuron for Multiple Neurons 

There is not much difference between 1D input space and high-dimensional input space. We just widen the input space. However, this simple trick will ease the interpretation of a neuron and the combination of several neurons.  

<blockquote style='background-color:#FFEFEF; font-style:normal;' markdown="1" >
**Formulation for an interpretation neuron: $$c=g(\mathbf{h})$$**
* $\mathbf{h}$ : an input of size $D$-dimension 
* $c$ : the concept to be verified for an input
* $g$ : a mapping from $h$ to $c$. 
* $W, \mathbf{b}$ : weight and bias for the classifier $g$. 
</blockquote >



Most previous work uses the representation of a layer for an input. The verification of a concept is done by learning a classifier whose input is a layer-representation and the output is a boolean for a specific concept. Note that when we can actually learn this, we may say there is a concept in activations. Even though this formulation has probed neurons with layer-representation, we don’t have to limit the function. We can learn a classifier whose input is the whole activation of a network. In this case, we can verify the neurons at once! 




## Probing 🧪

The verification of neurons is called *probing*. We probe the semantic information of  neurons. The overall algorithm is as follows:

**Step1**: Prepare samples
  * <span style="color:green"> Positive </span>  : Inputs which have concepts in its input space.  (Label 1) 
  * <span style="color:red"> Negative </span> : random or opposite conceptual images. (Label 0)  

**Step2**: Gather all activations of a network with samples in Step 1. 

**Step3**: Replace inputs in Step1 with the activations in Step2 

**Step4**: Train a classifier whose input is the activation score and the concept labels. 

**Step5**: Check the accuracy to see the pruning results.

**Step6**: Check the weight to verify the important neurons. 


### Linear Probing 

If you use a linear classifier in probing, it is called linear probing. 
The advantage of linear probing is the ease of weight interpretation.  As we want to verify the semantic information of neurons, we must know the relation between each neuron and the concept. The weight can provide this information

### Non-linear Probing  

If you use a non-linear classifier in probing, it is called linear probing. 
 The advantage of non-linear probing is that the concept could be complex and the linear relationship between neurons and concepts may not be enough, but the non-linear relationship may be.



## Interpret Chess Concepts ♜

Chess has many meaningful concepts in board states such as “Checkmate” and “Mobility“. 
Thomas et al. <d-cite key="mcgrath2022acquisition"/> investigate the concepts in chess with AlphaZero model.
Note that the network obtains only a board state and outputs preferred action and there is no explicit encoding of concepts. Therefore, the verification of neurons is about the existence of meaningful chess concepts in the internal representations. 

<figure style="text-align:center; display:block;width:100;">
<div style="display: flex; justify-content: center">
<img src="/assets/img/neurons_interpret_neurons/Interpret Chess Concepts.png" style="width:60%">
</div>
<figcaption>
Figure. Probing for human-encoded chess concepts in the AlphaZero network: generalize linear function $g(z^d)$ is trained to approximate $c(z^0)$.
</figcaption>
</figure>

<h4>What-When-Where plot for probing concepts.</h4>
<figure style="text-align:center; display:block;width:100;" >
<img src="/assets/img/neurons_interpret_neurons/chess_concept_king.png" style="width:100%"/>
<figcaption>
Figure. <strong>What-when-where</strong> (Checkmate-Training Step-Layer) plot to visualize the emergence of concepts in the network. Note that the z-axis is the accuracy of the probing network. As the training goes on, the concept of checkmate emerges. 
</figcaption>
</figure>
<figure style="text-align:center; display:block;width:100;" >
<img src="/assets/img/neurons_interpret_neurons/chess_concept_queen.png" style="width:100%"/>
<figcaption>
Figure. <strong>What-when-where</strong> (Queen-Training Step-Layer) plot to visualize the emergence of concepts in the network. As training goes on, the concept "Can capture queen" emerges. The emerged layers are deeper compared to the checkmate concept. 
</figcaption>
</figure>

The above accuracy is for binary classification. We can train a regression model to probe concepts. The Figures below show the probed continuous values while training. Note that as training goes on, the probed continuous values increased similar to the binary classification results. 


<figure style="text-align:center; display:block;width:100;" >
<img src="/assets/img/neurons_interpret_neurons/regression.png" style="width:100%">
<figcaption>
Figure. Regression probing. 
</figcaption>
</figure>


## Testing with Concept Activation Vector 🦓
 
The natural question followed by the concept is the usefulness of concepts.  
That is, how important the concept stripe is for the class zebra. 
Similarly, how much is the concept of red important for the class firefighter? 
To answer this question, we need a qualitative measure with concepts and classification logits. 
Testing with Concept Activation Vector (TCAV) <d-cite key="kim2018interpretability"/> is a measure that counts the number of samples that increased the class logic as the activation intervened with concept direction. 

Assume that we obtained the directional vector $v$  for the concept $C$ with probing. That is, $v_C^l$ is a weight in the probing classifier for concept $C$ and layer $l$. 
As we increase the activation pattern with the direction, we get more likelihood of concept $C$. 
That is, we can intervene the representation $f_l(x)$ in the direction of $v_C^l$,
$f_l(x) + \epsilon v_C^l$. TCAV utilizes the intervention to check whether it increases the classification logit.
This is a natural way of increasing concept. For example, the increased concept of stripe pattern will increase the logit of the zebra class. 
TCAV is a quantitative measure to this question and answers it with the ratio of samples which increases the logic. 

In summary: the classification logit will be increased if the amount of a concept increases.

### Conceptual Sensitivity of class k to concept C 

To quantitatively score the increased logit, the authors proposed Conceptual Sensitivity (CS) as follows:

$$ 
\begin{aligned}
S_{C,k,l}(\mathbf{x}) 
&= \lim_{\epsilon \rightarrow 0} \frac{h_{l,k}(f_l (\mathbf{x}) + \epsilon v_C^l) - h_{l,k}(f_l(\mathbf{x}))}{\epsilon} \\
&= \nabla h_{l,k}(f_l(\mathbf{x})) \cdot v_C^{l}
\end{aligned}
$$

where $h_{l,k}$ is the logit of class $k$ and $\mathbf{x}$ is input. Note that CS computes the alignment between the directional vector and the concept activation vector. 

### TCAV 

With CS score, the authors compute the validity of CS score over enough number of samples. 

$$
\mathrm{TCAV}_{Q_{C, k,l}} = \frac{\vert \{ x\in X_k : S_{C,k,l} (x) > 0  \}\vert }{\vert X_k \vert}
$$

where $X_k$ is the collection of samples with label $k$ such as collection of zebra images. Interpretation of TCAV results can be done as follows:

* 📌 If the concept has nothing to do with the class, the score will be low
* 📌 If the TCAV score has too high variance, we can not believe it <br>(requires additional statistical tests)
* 📌 **If the TCAV score is high with low variance, <br> we can say the concept increases the class logit.**


<figure style="text-align:center; display:block;width:100;" >
<img src="/assets/img/neurons_interpret_neurons/tcav.png" style="width:100%">
<figcaption>
Figure. TCAV. results for two classes and three concepts respectively. (*) indicates the failure cases for the statistical test. Note that: <br>
<strong>(1)</strong> Red has a higher TCAV score than other concepts in the fire engine class. <br>
<strong>(2)</strong> Only specific modules encode stripe patterns for the zebra class.  <br> © Copyright: TCAV paper 
</figcaption>
</figure>

## Conclusion 

In this section, we study a way to interpret neurons (conceptual representation) with other neurons (probing classifier). The probing technique is widely used to verify the semantic information in neural networks. Recently, Kenneth<d-cite key="li2022emergent"/> applied both linear and non-linear probings to verify the existence of conceptual board state in GPT. We believe more techniques utilizing neurons to interpret neurons in trained neural networks will be developed. 






