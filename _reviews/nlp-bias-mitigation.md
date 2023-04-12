---
layout: post
title: 'Bias Mitigation in NLP'
date: 2022-05-08 00:00:00-0400
description: ''
category: NLP
subcategory : Bias
---





## Debias


---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">Bias-Only Model</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">Focal Loss</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2020</tag>

Mahabadi [1] proposed directly trained biased model and reduce the training loss of the base model if the biased model predicts well. 
To reduce the loss effect, the authors propose two methods, Product of Experts (PoE) and Debiased Focal Loss (DFS). 

#### PoE and DFL 

`PoE` is defined by 
$$
\begin{gather}
f_C (x_i, x_i^b) = \log (\sigma(f_B(x_i^b))) + \log (\sigma(f_M(x_i))) \\
\mathcal{L}_{C}(\theta_M; \theta_B) = -\frac{1}{N} \sum_{i=1}^N \log (\sigma(f_C^{y_i}(x_i, x_i^b)))
\end{gather}
$$


* If biased-model correctly predicts, the base model gets no training signal
* If biased-model fails to classify, the gradient signal flows to the base model. 


`DFL` is defined by 
$$
\mathcal{L}_{C}(\theta_M; \theta_B) = -\frac{1}{N} \sum_{i=1}^N \Big(1- \sigma(f_B^{y_i}(x_i^b))\Big)^\gamma \log (\sigma(f_C^{y_i}(x_i)))
$$

* DFL downweights the training loss for biased samples. Therefore, only hard samples affect the training. 

#### Joint Debiasing 

The authors propose methods for combining several bias-only models as a neural network is prone to multiple types of biases in the datasets. 
Let $\{ \mathbf{x}_i^{b_j} \}_{j=1}^K be different sets of biased features of $\mathbf{x}_i$ that are predictive of $y_i$, and let $f_{B_j} be an individual bias-only model capturing $\mathbf{x}_i^{b_j}$. 


`Joint PoE`
The previous PoE is defined with single bias $x_i^b$ 

$$
f_C (x_i, x_i^b) = \log (\sigma(f_B(x_i^b))) + \log (\sigma(f_M(x_i^b)))
$$

The Joint PoE is defined by 

$$
f_C (x_i, \{ \mathbf{x}_i^{b_j} \}_{j=1}^K) = \textcolor{green}{\sum_{j=1}^K \log (\sigma(f_B(x_i^{b_j})))} + \log (\sigma(f_M(x_i)))
$$

`Joint DFL`

For the joint DFL, the authors averaged the DFL for predictions of individual bias-only models.

$$
f_B(\{x_i^{b_j}\}_{j=1}^K ) = \frac{1}{K} f_{B_j} (x_i^{b_j})
$$

Then compute, 

$$
\mathcal{L}_{C}(\theta_M; \theta_B) = -\frac{1}{N} \sum_{i=1}^N \Big(1- \sigma(\textcolor{greens}{f_B(\{x_i^{b_j}\}_{j=1}^K )} )\Big)^\gamma \log (\sigma(f_C^{y_i}(x_i)))
$$


### What is the Biased-Model 

* FEVER [dataset description]() : bias-only model predicts the labels using only claims as input. 
* SNLI and MNLI : bias-only model predicts the labels using only hypothesis. (results for both all and hard examples.)
* HANS (Syntactic Bias) : bias-only model uses 5 features
  1. whether all words in the hypothesis are included in the premise 
  2. If the hypothesis is the contiguous subsequence of the premise
  3. If the hypothesis is a subtree in the premise's parse tree
  4. The number of tokens shared between premise and hypothesis normalized by the number of tokens in the premise
  5. The cosine similarity between premise and hypothesis's pooled token representations from BERT

<Blockquote>
In the case of Joint model, syntactic heuristics and hypothesis only models are used. 
</Blockquote>

<hr/>


##

---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">???</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">NeurIPS (Workshop)</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2022</tag>

Generating Intuitive Fairness Specifications for Natural Language Processing



---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">???</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">ACL</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2022</tag>

Perturbation Augmentation for Fairer NLP


---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">???</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">ACL</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2021</tag>

Gender Bias in Machine Translation


---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">???</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">AAAI</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2022</tag>


Interpreting gender bias in neural machine translation: Multilingual architecture matters



---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">???</tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">ACL</tag>
<tag class="box-demo-link" style="background:#3549F3; color:#FFFFFF; font-size:18px">2020</tag>

Fine-tuning neural machine translation on gender-balanced datasets





# References 

[1] Mahabadi, Rabeeh Karimi, Yonatan Belinkov, and James Henderson. "End-to-End Bias Mitigation by Modelling Biases in Corpora." Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics. 2020.