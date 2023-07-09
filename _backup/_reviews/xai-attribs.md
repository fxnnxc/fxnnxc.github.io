---
layout: post
title: 'Attribution Methods in XAI [⚙️]'
date: 2023-04-09 00:00:00-0400
description: 'Attrib'
tags: XAI, Attrib
category: XAI
subcategory : Attrib
---


### Deconvolutional Networks

If the input to the ReLU during the forward pass is negative, then gradient signal is zero'd out. 

* Can not highlight negative signals

### Guided Backpropagation

zero's out the importance signal at a ReLU if either the input to the ReLU during the forward pass is negative or the importance signal during the backward pass is negative. 


* Can not highlight negative signals


---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Reference</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">DeepLIFT</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2018</tag>



### DeepLIFT 


DeepLIFT (Deep Learning Important FeaTures) assigns importance score to the inputs for a given output. 

*  importance in terms of differences from a 'reference' state 
* positive and negative contributions are modeled. 


#### Notations 

* $t$ : target output neuron of interest 
* $x_1, \cdots , x_n$ : some neurons in some intermediate layer or layers that are necessary and sufficient to compute $t$.
* $\Delta t = t - t^\mathrm{ref}$ : the difference-from-reference
* $C_{\Delta x_i \Delta t }$ : DeepLIFT contribution scores
* Summatation-to-delta 
$$
\sum_{i=1}^n C_{\Delta x_i \Delta t } = \Delta t
$$


#### Multipliers 

Given two neurons $x,t$, we wish to compute the contribution to, 

$$
m_{\Delta x \Delta t} = \frac{C_{\Delta x \Delta t }}{\Delta x}
$$

$m_{\Delta x \Delta t}$ is the contribution of $\Delta x$ to $\Delta t$ divided by $\Delta x$. 


#### Chain Rule 

Given input neurons $x_1, \cdots, x_n$, a hidden layer with neurons $y_1, \cdots, y_n$, and some target output $t$. 

$$
m_{\Delta x_i \Delta t} = \sum_j m_{\Delta x_i \Delta y_j} m_{\Delta y_i \Delta t}
$$

---


# Transformer Interpretability 

<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Rollout</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">ACL</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2020</tag>
<text style='color:#FF0000'> [1]  </text>


$$
E_{rawAttn} = \mathbb{E}_l (\alpha ^{(L)})
$$


$$
\begin{gather}
A^{(1)} = I + \mathbb{E}_h (\alpha^{(l)}) \\
E_{rollout} = A^{(1)} \cdot A^{(2)} \cdots A^{(L)}
\end{gather}
$$

---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Partial LRP</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">ACL</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2019</tag>
<text style='color:#FF0000'> [2]  </text>

<text style='color:#FF0000'> /  </text>
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Transformer Attribution</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">CVPR</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2021</tag>
<text style='color:#FF0000'> [3]  </text>



### Partial-LRP


$$
E_{partialLRP} = \mathbb{E}_h (R^{(L)})
$$


### Transformer Attribution

$$
\begin{gather}
A^{(l)} = I + \mathbb{E}_h (\alpha^{(l)} \odot R^{(l)})^{+} \\
E_{TransAttn} = A^{(1)} \cdot A^{(2)} \cdots A^{(L)}
\end{gather}
$$




---

<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Generic</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">CVPR</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2021</tag> 
<text style='color:#FF0000'> [4]  </text>


$$
\mathbf{A}^{(l)} = \mathbb{E}_h ((\nabla \alpha^{(l)} \odot \alpha^{(l)} )^{+}) 
$$

$$
\begin{gather}
E_{AttnGrads} = A^{(1)} \cdot A^{(2)} \cdots A^{(L)}
\end{gather}
$$

---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">AttGrad</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2021</tag> 
<text style='color:#FF0000'> [5]  </text>




$$
\mathbf{A}^{(l)} = \mathbb{E}_h (\nabla \alpha^{(l)} \odot \alpha^{(l)} ) 
$$

$$
\begin{gather}
E_{AttnGrads} = A^{(1)} \cdot A^{(2)} \cdots A^{(L)}
\end{gather}
$$



---

<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">AttnCAT</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">NeurIPS</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2022</tag>
<text style='color:#FF0000'> [6]  </text>

### CAT

$$
\begin{gather}
\mathbf{A}^{(l)} = \mathbb{E}_h (\nabla h^{(l)} \odot h^{(l)} )  \\
E_{AttnGrads} = A^{(1)} + \cdots  + A^{(L)}
\end{gather}
$$

### AttnCAT

$$
\begin{gather}
\mathbf{A}^{(l)} = \mathbb{E}_h  \alpha^{(l)} \odot (\nabla h^{(l)} \odot h^{(l)} )  \\
E_{AttnGrads} = A^{(1)} + \cdots  + A^{(L)}
\end{gather}
$$


---







---
# References 


[1] Abnar, Samira, and Willem Zuidema. "Quantifying Attention Flow in Transformers." Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics. 2020.

[2] Voita, Elena, et al. "Analyzing Multi-Head Self-Attention: Specialized Heads Do the Heavy Lifting, the Rest Can Be Pruned." Proceedings of the 57th Annual Meeting of the Association for Computational Linguistics. 2019.

[3] Chefer, Hila, Shir Gur, and Lior Wolf. "Transformer interpretability beyond attention visualization." Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition. 2021.

[4] Chefer, Hila, Shir Gur, and Lior Wolf. "Generic attention-model explainability for interpreting bi-modal and encoder-decoder transformers." Proceedings of the IEEE/CVF International Conference on Computer Vision. 2021.

[5] Barkan, Oren, et al. "Grad-sam: Explaining transformers via gradient self-attention maps." Proceedings of the 30th ACM International Conference on Information & Knowledge Management. 2021.

[6] Qiang, Yao, et al. "Attcat: Explaining transformers via attentive class activation tokens." Advances in Neural Information Processing Systems. 2022.