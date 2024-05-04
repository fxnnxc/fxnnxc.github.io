---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-04-29
featured: true
title: 'Early Experiments on Sparse Autoencoding (SAE)'
description: '신경망의 표현에 숨어있는 수많은 특징들을 찾아내기 위해서 최근 연구되는 SAE 기법을 추종하기 위해 실험을 진행하였다. '
---

모델의 표현을 해석한다는 것은 특정 액티베이션 패턴이 지니는 의미를 데이터와 연결 짓는 것이다. 최근 알려진 바에 따르면 모델이 특징을 저장하기 위해서 복수 개의 뉴런이 특정 패턴을 만들어서 저장한다는 것이 알려졌다. 이 사실은 기존에 binary적인 뉴런의 켜지고 꺼지는 현상으로 특징의 유무를 판단했던 것을 넘어서 특정 액티베이션 패턴이 의미를 지닌다는 사실을 나타낸다. 축약된 차원에 저장된 무수히 많은 특징을 찾기 위해서 연구자들이 사용하는 방식은 dictionary learning이다. 특징인 패턴을 집어넣고, 다시 복원하는 간단한 문제에서 내부에 무수히 많은 feature들을 저장하고 선택하는 방식은, 그들의 linear sum이 신경망의 특징을 나타낸다는 가정을 보인다. 

나는 이 연구를 추종하기 위해서 코드를 구현하고 실험하였다. Wikipedia 10만개 문서에 대해서 Llama2의 activation을 수집하여 SAE 학습을 진행하였다. 
이 부분은 그 길에서 가장 간단한 구조로부터 무엇이 잘되고 안되는지 파악하기 위한 연구였다. 실험 결과, 모델의 층이 높아질수록 Reconstruction이 제대로 일어나지 않았고, GatedSAE는 더 낮은 성능을 보였다. 이는 Neuron Resampling기법을 적용하지 않았기 때문에 그런 것이다. 


* 🧑🏻‍💻 Code Implementation [[Github](https://github.com/fxnnxc/llm/tree/v24.05.05_SAE)]
* 🧑🏻‍💻 PseudoCode Implementation [[Gist](https://gist.github.com/fxnnxc/35a72b2af899a6f339bb9d2aa09ab563)]


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

### 1. Reconstruction

표현을 Dictionary learning으로 결합하였을 때 복원력

$$
\Vert x - \hat{x}(f(x)) \Vert_2^2 
$$

### 2. L0 
표현을 Dictionary learning으로 결합하였을 때 복원력에 필요했던 아이템 개수 

L0 be the number of non zero features  

$$L0 = \mathbb{E}_{X\sim D} \Vert f(x)\Vert_0$$  

### 3. loss recovered 

표현을 Dictionary learning으로 결합하였을 때 복원력 (모델 성능 기반)

$$
1 - \frac{CE(\hat{x} \cdot \hat{f}) - CE(ID)}{CE(\psi) - CE(ID)}
$$

where 
* $x \mapsto x$ :  the identity function, $\psi: x \mapsto 0$ the zero-ablation and $\hat{x} \cdot \hat{f}$ is autoencoder.
* When the ID is recovered with SAE, the score is 1. 

## 데이터 

* LLama2 Activations 
* Layers: 2, middle, -2  (early, and later)
* Wikipedia: 100,000개. (from fever dataset)

---

## 초기 실험 결과 

1. lr_scheduler: `cosine hard restart`를 사용하는 경우, 학습에 효과가 좋지 않다. 
2. gatedSAE의 경우, SAE보다 성능이 좋지 않다. (논문에서는 Neuron Resampling을 하였다고 한다. 이 부분이 성능 차이를 보인 것으로 보인다. 왜냐하면, gate가 켜지는 경우, gate에 Bias가 되기 때문이다. )
3. early, middle, later에 대해서 앞의 두 개는 학습이 제대로 되었지만, 마지막은 학습되지 않았다. 
4. L1 coefficient는 하는 만큼 Reconstruction에 영향을 미친다. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%218851&authkey=%21ABvIo6RPLoT1Oao&width=746&height=578" width="746" height="578" />

### 왜 레이어가 올라갈수록 Reconstruction은 되지 않는가? 

특징이라는 것은 결정적이고, 고정적인 성질을 지녀야 한다. 다음과 같은 사고 실험을 생각해보자. 

1. 레고를 여러 개 보여준다. (input batch)
2. 레고를 구성하는 컴포넌트들을 고르라고 한다. (items)
3. 컴포넌트들을 결함하여 각 레고들을 모두 복원하라고 한다 (selection)
4. 원래 레고들과 각각 비교한다. (reconstruction)

전체 과정에서 2, 3번은 딥러닝의 End-to-End 학습을 통해서 적절한 Item 표현과 selection을 찾는 과정이다. 
그 과정에서 Item의 표현이 적절하지 않거나 selection부분이 적절하지 않을 수 있다. 

레이어가 높아질수록 학습되지 않았던 것은 두 가지 모두 영향을 끼치는 것으로 확인된다. 
즉, 레이어가 올라가면서 다음 문제가 생긴다. 

<blockquote>
복잡한 레이어의 표현공간에 대해서 적절한 <strong>Primitive component</strong>를 찾을 수 있는가? (No)
</blockquote>
