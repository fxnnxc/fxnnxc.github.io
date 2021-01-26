---
title: '[논문 리뷰] Learning Structured Output Representation using Deep Conditional Generative Models'
date: 2021-01-22
permalink: /posts/2021/01/paper-conditional-generative/
tags:
  - CNN
  - VAE
  - CVAE
---

# Learning Structured Output Representation using Deep Conditional Generative Models

## Abstract
* it is still challenging to model com- plex structured output representations that effectively perform probabilistic infer- ence and make diverse predictions
* a deep conditional generative model for structured output prediction using Gaussian latent variables
* input noise-injection and multi-scale prediction objective at training.

## Introduction
* In structured output prediction, it is important to learn a model that can perform probabilistic in- ference and make diverse predictions.
* deep conditional generative models (CGMs) for output representation learning and structured prediction
* a conditional variational auto-encoder (CVAE)

## Related work
* pass 

## Preliminary: Variational Auto-encoder
* VAE에 대한 기본적인 설명.

## Deep Conditional Generative Models for Structured Output Prediction

$$
\log{p_{\theta}(\mathbf{y}|\mathbf{x})} \geq -\it{KL}(q_{\phi}(\mathbf{z}|\mathbf{x},\mathbf{y}) ||p_{\theta}(\mathbf{z}|\mathbf{x})) + \mathbb{E}_{q_{\phi}(\mathbf{z}|\mathbf{x},\mathbf{y})} [\log p_{\theta}(\mathbf{y}|\mathbf{x},\mathbf{z})]
$$


$$
\tilde{\mathcal{L}}_{CVAE}(\mathbf{x},\mathbf{y};\theta, \phi) = -\it{KL}(q_{\phi}(\mathbf{z}|\mathbf{x},\mathbf{y}) ||p_{\theta}(\mathbf{z}|\mathbf{x})) + \frac{1}{L}\sum_{l=1}^{L} \log p_{\theta}(\mathbf{y}|\mathbf{x},\mathbf{z})
$$



$\text{ where } \mathbf{z}^{(l)} = g_\phi(\mathbf{x}, \mathbf{y},\epsilon^{(l)}), \epsilon^{(l)} \sim \mathbin{N}(\mathbf{0},\mathbf{I})$

$L$ is the number of samples. 
CVAE 
* recognition network $q_\phi(z|x,y)$
* (conditional) prior network $p_\theta(z|x)$
* generation network $p_\theta(y|x,z)$
* They build it on top of the baseline $\text{CNN}$


![images](/images/posts/CGM1.png)







## CVAE for image segmentation and labeling
* multi-scale prediction
  * global-to-local, coarse-to-fine-grained prediction이 가능하다. 

![images](/images/posts/CGM2.png)

* training with input omission noise
  * Adding noise to neurons.
  * Similary, wemantic segmentation을 위한 regularization technique
    * Corrupt the input data $\mathbf{x}$ into $\tilde{\mathbf{x}}$
    * Optimize the nwtwork with the following objective: $\tilde{\mathcal{L}}(\tilde{\mathbf{x}}, \mathbf{y})$
    * Randomly generate a squared mask of width and heigh less than 40% of the image width and height, at random position and set pixel values of the input image inside the mask to . 

## Experiments 
* Toy example : **MNIST**

![images](/images/posts/CGM3.png)
![images](/images/posts/CGM4.png)


* **Caltech-UCSD Brids(CUB)**
  * 6,033 images of birds from 200 species with annotations such as a bounding box of birds and a segmentation mask. 
  * Train/Test : as suggested in the other paper
  * 10 folds cross-validation

![images](/images/posts/CGM5.png)
---
![images](/images/posts/CGM6.png)
---
## Conclusion
stochastic neural networks for structured output prediction based on the conditional deep generative model with Gaussian latent variables. The