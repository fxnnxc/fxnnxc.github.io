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

* Athors
  * Kihyuk Sohn 
  * Honglak Lee 
  * Xinchen Yan


## Abstract

* 복잡한 모델의 아웃풋에 대한 효율적이고 확률적인 표현이 어려웠고, 다양한 아웃풋을 얻는 추론하는 게 쉽지 않았습니다.  
* 가우시안 잠재 변수를 사용해서 ```Deep Contitional Generative Model```에 대한 구조적인 아웃풋을 가능하게 한 게 이 논문의 주요 내용입니다.
* **input noise-injection** : input 데이터에 노이즈를 추가해서 regularization을 하는 방법입니다. 
* **multi-scale prediction objective** : 마지막 층에 가기 전에 단계별로 추론을 하고, Loss를 더하는 방법입니다. 

## Introduction
* Structured Output Prediction에서는 확률적인 추론과 다양한 예측이 중요합니다. 여기서 말하는 Structured Output Prediction은 단순히 숫자를 예측하는 것을 넘어서 구조 자체를 예측하는 것을 말합니다. 
* Deep conditional generative models (CGMs) 
* A conditional variational auto-encoder (CVAE)

## Related work & Preliminary: Variational Auto-encoder
* pass 

## Deep Conditional Generative Models for Structured Output Prediction


$$
\log{p_{\theta}(\mathbf{y}|\mathbf{x})} \geq -\it{KL} (q_{\phi}(\mathbf{z}|\mathbf{x},\mathbf{y}) ||p_{\theta}(\mathbf{z}|\mathbf{x})) + \mathbb{E}_{q_{\phi}(\mathbf{z}|\mathbf{x},\mathbf{y})} [\log p_{\theta}(\mathbf{y}|\mathbf{x},\mathbf{z})]
$$

VAE의 ELBO와 비슷하게, CVAE에서도 ELBO를 학습시킵니다. 우변의 두 번째 값은 intractable하므로 아래와 같이 Loss를 적을 수 있습니다. 



$$
\tilde{\mathcal{L}}_{CVAE}(\mathbf{x},\mathbf{y};\theta, \phi) = -\it{KL}(q_{\phi}(\mathbf{z}|\mathbf{x},\mathbf{y}) ||p_{\theta}(\mathbf{z}|\mathbf{x})) + \frac{1}{L}\sum_{l=1}^{L} \log p_{\theta}(\mathbf{y}|\mathbf{x},\mathbf{z})
$$



$\text{ where } \mathbf{z}^{(l)} = g_\phi(\mathbf{x}, \mathbf{y},\epsilon^{(l)}), \epsilon^{(l)} \sim \mathbin{N}(\mathbf{0},\mathbf{I})$

$L$ is the number of samples. 
CVAE 
* (c) recognition network $q_\phi(z\bar x,y)$
* (b) (conditional) prior network $p_\theta(z\|x)$
* (b) generation network $p_\theta(y\|x,z)$
* (a) They build it on top of the baseline $\text{CNN}$


![images](/images/posts/CGM1.png)


## CVAE for image segmentation and labeling
* multi-scale prediction
  * global-to-local, coarse-to-fine-grained prediction이 가능하다. 
  * 마지막 단계뿐만 아니라, 레이어 중간에서도 Prediction을 예측하는 방법
  
![images](/images/posts/CGM2.png)

* training with input omission noise
  * Adding noise to neurons.
  * Similary, wemantic segmentation을 위한 regularization technique
    * Corrupt the input data $\mathbf{x}$ into $\tilde{\mathbf{x}}$
    * Optimize the nwtwork with the following objective: $\tilde{\mathcal{L}}(\tilde{\mathbf{x}}, \mathbf{y})$
    * Randomly generate a squared mask of width and heigh less than 40% of the image width and height, at random position and set pixel values of the input image inside the mask to . 

## Experiments 
* Data : Toy example : **MNIST**
  * 사분면에 대해서 Mask를 진행하고, Prediction을 하는 문제입니다. 
  * CVAE가 기본적인 모델보다 성능이 뛰어난 걸 확인 할 수 있습니다.  

![images](/images/posts/CGM3.png)
![images](/images/posts/CGM4.png)


* Data : **Caltech-UCSD Brids(CUB)**
  * 6,033 images of birds from 200 species with annotations such as a bounding box of birds and a segmentation mask. 
  * Train/Test : as suggested in the other paper
  * 10 folds cross-validation

![images](/images/posts/CGM5.png)
---

input 데이터에 대해서 omission noise를 추가하고 그 구조를 prediction하는 문제입니다. CNN 모델만 사용했을 때는, 예측하지 못했던 구조를 예측할 수 있는 것을 확인할 수 있습니다. 
![images](/images/posts/CGM6.png)

---
## Conclusion

이미지는 연속적인 특성을 가지고 있어서, VAE에 이를 활용한다면, **Latent Space**에 특징을 인코딩할 수 있습니다. CVAE에서는 Label과 input 데이터에 대한 정보를 바탕으로 $z \sim q(z|x,y)$를 추론하게 되는데, 단순히 input 데이터만으로 추론한 $\tilde{z} \sim p(z|x)$ 와 값이 가까워짐에 따라, Latent Space에 $y$에 대한 정보가 담기게 됩니다. 


## References 

[1] Learning Structured Output Representation using Deep Conditional Generative Models
https://papers.nips.cc/paper/2015/hash/8d55a249e6baa5c06772297520da2051-Abstract.html