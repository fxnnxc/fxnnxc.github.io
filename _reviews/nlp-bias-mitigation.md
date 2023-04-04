---
layout: post
title: 'Bias Mitigation in NLP'
date: 2022-05-08 00:00:00-0400
description: ''
category: NLP
subcategory : Bias
---


## Debias

<hr/>
Mahabadi [1] proposed directly trained biased model and reduce the training loss of the base model if the biased model predicts well. 
To reduce the loss effect, the authors propose two methods, Product of Experts (PoE) and Debiased Focal Loss (DFS). 

PoE is defined by 
$$
\begin{gather}
f_C (x_i, x_i^b) = \log (\sigma(f_B(x_i^b))) + \log (\sigma(f_M(x_i^b))) \\
\mathcal{L}_{C}(\theta_M; \theta_B) = -\frac{1}{N} \sum_{i=1}^N \log (\sigma(f_C^{y_i}(x_i, x_i^b)))
\end{gather}
$$


* If biased-model correctly predicts, the base model gets no training signal
* If biased-model fails to classify, the gradient signal flows to the base model. 


DFL is defined by 
$$
\mathcal{L}_{C}(\theta_M; \theta_B) = -\frac{1}{N} \sum_{i=1}^N \Big(1- \sigma(f_B^{y_i}(x_i^b))\Big)^\gamma \log (\sigma(f_C^{y_i}(x_i)))
$$

* DFL downweights the training loss for biased samples. Therefore, only hard samples affect the training. 


<hr/>



# References 

[1] Mahabadi, Rabeeh Karimi, Yonatan Belinkov, and James Henderson. "End-to-End Bias Mitigation by Modelling Biases in Corpora." Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics. 2020.