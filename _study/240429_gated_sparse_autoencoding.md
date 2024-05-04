---
layout: default
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-04-28
featured: true
title: 'Early Experiments on Gated Sparse Autoencoding (GSAE)'
description: 'Feature를 해석하기 위해서는 neuron의 패턴을 더 잘게 쪼개야 한다. 이를 학습하는 방법은 spare autoencoding이다. 이 실험에서는 neuron resampling을 구현하지 않아 뉴런 값이 크게 고정되어 실패하였다. '
---


* PseudoCode Implementation [[Gist](https://gist.github.com/fxnnxc/35a72b2af899a6f339bb9d2aa09ab563)]

## 아키텍처 

###  SAEs
$$
f(x) = ReLU(W_{enc} (x- b_{dec}) + b_{enc}) \\ 
\hat{x}(f) = W_{dec}f + b_{dec}
$$

$$
L(x) =  || x - \hat{x}(f(x)) ||_2^2  + \lambda ||f(x)||_1
$$

### Gated SAEs

$$
\hat{f}(x) = 1[W_{gate}(x-b_{dec} +b_{gate}) >0] \odot ReLU(W_{mag} (x- b_{dec}) + b_{mag})
$$

where $1[\cdot >0]$ is the pointwise Heavyside step function and $\odot$ denotes elementwise multiplication. 
To reduce the number of weights, the authors set the weight
$$
(W_{mag})_{ij} =  (\exp(r_{mag}))_i \cdot (W_{gate})_{ij}
$$
where $r_{mag} \in \mathbb{R}^M$ is the rescaling parameter. 

$$
L_{incorrect}(x) = L(x) =  || x - \hat{x}(f(x)) ||_2^2  + \lambda ||f_{gate}(x)||_1
$$



## 평가 


* $L0 = \mathbb{E}_{X\sim D} ||f(x)||_0$ : be the number of non zero features  
* loss recovered 
$$
1 - \frac{CE(\hat{x} \cdot \hat{f}) - CE(ID)}{CE(\ksi) - CE(ID)}
$$

where 
* $\x \mapsto x$ :  the identity function, $\ksi: x \mapsto 0$ the zero-ablation and $\hat{x} \cdot \hat{f}$ is autoencoder.
When the ID is recovered with SAE, the score is 1. 

## 데이터 



## 실험 결과 

| method   | 
|--------  |--------|--------|
| SAE      |
| GSAE     |

