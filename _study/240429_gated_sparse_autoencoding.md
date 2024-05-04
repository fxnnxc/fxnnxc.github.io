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
title: '[Draft] Gated Sparse Autoencoding (GSAE)'
description: '[baseline] 실험 구현 및 평가'
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

### Reconstruction

$$
\Vert x - \hat{x}(f(x)) \Vert_2^2 
$$

### L0 

L0 be the number of non zero features  

$$L0 = \mathbb{E}_{X\sim D} \Vert f(x)\Vert_0$$  

### loss recovered 

$$
1 - \frac{CE(\hat{x} \cdot \hat{f}) - CE(ID)}{CE(\psi) - CE(ID)}
$$

where 
* $x \mapsto x$ :  the identity function, $\psi: x \mapsto 0$ the zero-ablation and $\hat{x} \cdot \hat{f}$ is autoencoder.
When the ID is recovered with SAE, the score is 1. 

## 데이터 

* LLama2 Activations 
* Layers: 2, middle, -2  (early, and later)
* Wikipedia: 100,000개. (from fever dataset)
* 

---

## 초기 실험 결과 

1. lr_scheduler: `cosine hard restart`를 사용하는 경우, 학습에 효과가 좋지 않다. 
2. gatedSAE의 경우, SAE보다 성능이 좋지 않다. (논문에서는 Neuron Resampling을 하였다고 한다. 이 부분이 성능 차이를 보인 것으로 보인다. 왜냐하면, gate가 켜지는 경우, gate에 Bias가 되기 때문이다. )
3. early, middle, later에 대해서 앞의 두 개는 학습이 제대로 되었지만, 마지막은 학습되지 않았다. 
4. L1 coefficient는 하는 만큼 Reconstruction에 영향을 미친다. 
5. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%218851&authkey=%21ABvIo6RPLoT1Oao&width=746&height=578" width="746" height="578" />
