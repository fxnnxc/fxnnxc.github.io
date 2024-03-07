---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2024-02-31
featured: true
title: 'Positional Encoding'
description: ''
---

* 단어길이 $n$

입력 $x = (x_1, x_2, \cdots, x_n)$ 에 대해서 softmax 는 $z=(z_1,z_2, \cdots, z_n)$ 를 내보내며, 다음과 같이 쓰여질 수 있다. 

$$
z_i = \sum_{j=1}^n \frac{\exp (\alpha_{ij})}{\sum_{j^'=1}^n \exp (\alpha_{ij^'}} (x_j W^V)
$$
where 
$$
\alpha_{ij} = \frac{1}{\sqrt{d}}(x_i W^Q)(x_j W^V)^\top
$$


주어진 position 정보로 모델은 토큰 위치를 구분할 수 있다. 
absolute positional encoding 은 주어진 단어 임베딩 $w_i\in \mathbb{R}^d$에 position 정보 $p_i\in \mathbb{R}^d$를 더하는 방식으로 연산된다. 

$$
x_i = w_i + p_i 
$$
이로부터 self-attention 모듈은 다음과 같이 쓰여질 수 있다. 

$$
\alpha_{ij} = \frac{1}{\sqrt{d}}((w_i + p_i) W^Q)((w_j + p_j) W^V)^\top
$$

주어진 $p_i$ 는 미리 정해질수도 있고, 학습으로 바뀔 수도 있다. 관건은 $p$를 설계하는 방식에 의해서 어떤 특징이 나타나는지 이해해야 한다. position은 벡터를 전혀다른 공간으로 맵핑하는지 아니면 잊혀지는지 알고 싶다. 물론 sinusoidal positional encoding의 경우, 차원의 뒷부분으로 갈수록 시그널 자체가 약해지므로 앞 부분이 position에 영향을 받는 공간을 형성한다. 

상대적인 단어의 거리를 고려하지 못하는 단점이 있다. (Shaw et al. 2018)

$$
\alpha_{ij} = \frac{1}{\sqrt{d}}((x_i) W^Q)((x_j + a_{j-i}) W^V)^\top
$$

where $a_{j-i} \in \mathbb{R}^d$ is learnable parameter. 

T5 (Rafael 2019) 에서는 relative position을 query key 연산에서 제거하였다. 

$$
\alpha_{ij} = \frac{1}{\sqrt{d}}((x_i) W^Q)((x_j) W^V)^\top + b_{j-i}
$$

여기서 $b_{j-i} \in \mathbb{R}$ 은 scalar이다.



---


Sinusoidal 의 가정 

$$
PE(t, 2i) = \sin (t \cdot \frac{1}{10000}^{\frac{2i}{d}}) \\
PE(t, 2i+1) = \cos (t \cdot \frac{1}{10000}^{\frac{2i}{d}}) 
$$

* 최소 dimension : $\sin (t \cdot \frac{1}{10000}^{0}) = sin(t) $ 
* 최대 dimension : $\sin (t \cdot \frac{1}{10000}^{1}) = sin(\frac{1}{10000}t) $ 

따라서, $2\pi$ 부터 $2\pi \cdot 10000$ 까지 wavelength가 존재하도록 가정. 
고차원으로 갈수록 position에 대해서 크게 변하지 않는 vector 를 가진다. 