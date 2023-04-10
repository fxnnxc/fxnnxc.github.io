---
layout: post
title: 'Explainable Agent [⚙️]'
date: 2023-04-08 00:00:00-0400
description: 'Agent'
tags: XAI, Agent
category: XAI
subcategory : Agent
---

# Explainable Agent


 The goal of this literature review is for the idea that 

 <Blockquote>
Deep Neural Network can be interpreted by another Deep Neural Network. 
 </Blockquote>


---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Agent</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">Self-Talk</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2022</tag>

Roy et al. [1] proposed self-talk agent which is an explanatory model. 



### Desiderata for explanatory model 

<b style='border:1px; background-color:#FFFFBB;'> Grounded  </b>
* The explanatory model should deploy a representation language that is **semantically-grounded** 
* (NLP description or One-hot Encoding)

<b style='border:1px; background-color:#FFBBFF;'> Flexible  </b> 
* Be able to produce many different representation forms, **agnostic to the base system's input/output** modalities.
* (Most internal representations are not interpretable.)

<b style='border:1px; background-color:#BBFFFF;'> Minimally-interfering  </b> 
* Training and inference of the model should have as **little impact as possible** on the training and inference of the base system. 
* (Attribution methods are post-hoc)

<b style='border:1px; background-color:#BBBBFF;'> Scalable  </b>
* The method should be adapted to **any size network** 
* (Saliency map can be applied to any differentiable model.) 

<b style='border:1px; background-color:#BBFFBB;'> Faithful  </b>  
* The explanatory model should provide **accurate descriptions** of the underlying system 
* (Is saliency map accurate? May be not) 


### Common historical solutions [1]

|---|---|---| 
|Method| <tag style='color:green'> Satisfied </tag>| <tag style='color:red'> Unsatisfied </tag>| 
|**Do Nothing**|<b style='border:1px; background-color:#BBFFFF;'> Minimally-interfering  </b>, <b style='border:1px; background-color:#BBBBFF;'> Scalable  </b> , <b style='border:1px; background-color:#BBFFBB;'> Faithful  </b>| <b style='border:1px; background-color:#FFFFBB;'> Grounded  </b>, <b style='border:1px; background-color:#FFBBFF;'> Flexible  </b>|
|**Attribution Methods**|<b style='border:1px; background-color:#BBFFFF;'> Minimally-interfering  </b> (post-hoc), <b style='border:1px; background-color:#BBBBFF;'> Scalable  </b> (automated)| <b style='border:1px; background-color:#BBFFBB;'> Faithful  </b>, <b style='border:1px; background-color:#FFFFBB;'> Grounded  </b>, <b style='border:1px; background-color:#FFBBFF;'> Flexible  </b> (particular form)|
|**Explicitly structuring networks**| <b style='border:1px; background-color:#FFFFBB;'> Grounded  </b>, <b style='border:1px; background-color:#BBFFBB;'> Faithful  </b> | <b style='border:1px; background-color:#BBFFFF;'> Minimally-interfering  </b>, <b style='border:1px; background-color:#FFBBFF;'> Flexible  </b>, <b style='border:1px; background-color:#BBBBFF;'> Scalable  </b> |
|**Fine-grained mechanistic interpretability**| <b style='border:1px; background-color:#BBFFFF;'> Minimally-interfering  </b>, <b style='border:1px; background-color:#BBFFBB;'> Faithful  </b> |<b style='border:1px; background-color:#BBBBFF;'> Scalable  </b> , <b style='border:1px; background-color:#FFFFBB;'> Grounded  </b>, <b style='border:1px; background-color:#FFBBFF;'> Flexible  </b>|
|**Decoders**|<b style='border:1px; background-color:#FFFFBB;'> Grounded  </b>,<b style='border:1px; background-color:#FFBBFF;'> Flexible  </b>, <b style='border:1px; background-color:#BBBBFF;'> Scalable  </b> , <b style='border:1px; background-color:#BBFFFF;'> Minimally-interfering  </b> | <b style='border:1px; background-color:#BBFFBB;'> Faithful  </b> |
|**Causal Self-Talk**| <b style='border:1px; background-color:#FFFFBB;'> Grounded  </b>,<b style='border:1px; background-color:#FFBBFF;'> Flexible  </b>, <b style='border:1px; background-color:#BBBBFF;'> Scalable  </b> , <b style='border:1px; background-color:#BBFFFF;'> Minimally-interfering  </b>, <b style='border:1px; background-color:#BBFFBB;'> Faithful  </b> | -|





#### Notations 

* $x_t, m_t, \pi_t, z_t$ : input, constructed message, policy, and decoder output at time $t$.  
* $m_{t+1} = f_\theta([x_{t+1}, z_t], m_t)$ : next message given  $m_t$, $x_{t+1}$, and $z_{t}$.
* $\pi_t = h_\theta(m_t)$ : policy from message $m_t$



### Re-routed Decoder

To route the output of the decoder back into the internal representation, e.g. by concatenating $z_t$ with $x_{t+1}$. This amounts to incorporating the decoder output in the state update rule, with $m_{t+1} = f_\theta([x_{t+1}, z_t], m_t)$.

However, there is no guarantee that $f_\theta$ will actually "listen" to $z_t$ when producing $m_{t+1}$. The decoder pathway can be viewed as a (externally-grounded) communication channel between the internal state and itself, i.e. a form of "self-talk" or "inner speech" [2].


### Causal self-talk

<Blockquote>
Intervene the memory and rely on the decoder output to (maximize return, reconstruct memory, or distill the policy ). 
For example, internal $m_4$ is reverted to $m_1$, so that the (true, online) message $z_3$ must inform the agent accordingly. Note that only $z_3$ has information of $m_2$ and $m_3$ as we intervene $m_4$ with $m_1$. 
$$
\Large{\textcolor{blue}{m_1}, z_1, m_2, z_2, m_3, \textcolor{green}{z_3}, \textcolor{red}{m_4}}
$$


</Blockquote>


To compactly and effectively communicate information to earlier versions of the same agent (from the same episode), via a semantically-grounded communication channel. 
Formally, this takes the form of an intervention internal state, $m_t$, is replaced with a counterfactual one, $m_{t'}$ , before being integrated with the
original message ($z_t$) and next observation $(x_{t+1})$, in order to produce a counterfactual internal state, 
$$
m^{(t\leftarrow t')}_{t+1} = f ([x_{t+1}; z_t];m_{t'} )
$$

This yields a counterfactual policy $\pi_{t+1}^{(t\leftarrow t')} = h_\theta (m_{t+1}^{(t\leftarrow t')})$ and 

* CST-RL : Maximizing the return. 
* CST-MR : memory reconstruction $$\mathcal{L}_\mathrm{MR} = \lVert  m_{t+1}^{(t\leftarrow t')} - m_{t+1} \rVert$$
* CST-PD : policy distillation  $$\mathcal{L}_\mathrm{PD} = \sum_{\Delta t > 0} \gamma (\Delta t) \cdot D_{KL} (\pi_{t+\Delta t} \Vert \pi_{t+ \Delta t}^{(t\leftarrow t')})$$ where $\gamma$ is a discounting term 





---

# References 


[1] Roy, Nicholas Andrew, Junkyung Kim, and Neil Charles Rabinowitz. "Explainability Via Causal Self-Talk." Advances in Neural Information Processing Systems.


[2] L S Vygotsky. Thought and Language. The MIT Press. MIT Press, London, England, 2012.

[3] Inducing causal structure for interpretable neural networks.