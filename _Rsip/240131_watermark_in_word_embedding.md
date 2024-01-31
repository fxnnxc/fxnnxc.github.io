---
layout: share-distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2024-01-31
featured: true
img: 
title: 'Watermark in Word Embedding'
description: ''
_styles: >
    .table {
        padding-top:200px;
        margin-bottom: 2.5rem;
        border-bottom: 2px;
    }
    .p {
        font-size:20px;
    }

---

## Watermark in Word Embedding


주어진 단어 임베딩 공간 $E=\{e_1, e_2, \cdots, e_V}$에 대해서 문서 워터마크 임베딩$W=\{w_1, w_2, \cdots, w_M\}$ where $w_j \sim \mathcal{N}(0, \sigma)$ 은 단어 임베딩 공간에 대해서 충분히 작은 variance 를 가져야 한다. 임베딩 $e_i$ 와 문서 워터마크 $w_j$ 를 입력에 넣는 경우를 가정하자. 

$$
\hat{e}_i = e_i + w_j 
$$

만일 $w_j$ 의 크기가 $e_i$ 들을 구분하는 거리보다 크다면, 랜덤하게 뽑은 $w_j$들은 다른 단어의 임베딩을 침범하게 된다. 따라서 문서 워터마크에 대해서는 다음과 같은 크기의 제약이 필요하다. 

$$ 
|e_k -  e_i | >  |w_j| 
$$

아래 그림은 단어 임베딩 공간에서 문서 워터마크의 크기에 따른 영역 침범을 보여준다. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21924&authkey=%21AKqZxG_oAW8Ovmg&width=593&height=422" width="593" height="422" />

GPT를 학습하는 과정에서 단어 임베딩 공간은 업데이트 되므로 고정 값이 아니다. 이에 반해 문서 워터마크는 고정값을 가정하고 있다. 따라서 학습 과정에서 문서 워터마크를 넣는다면, 애시당초 충분히 작은 값을 고정적으로 줘야 한다. (물론, 문서 워터마크 표현도 differential 하게 학습하는 방식을 고려할 수 있을 것이다.)

이 경우, 가능한 방식은 문서 워터마크가 the unit $d-1$-sphere 에 존재하며, 충분히 작은 $\alpha$ 를 가진 공간에 놓는 것이다. 이는 기존 연구의 관점과 동일하다. 적절한 magnitude 에 대해서는 아직 밝혀내지 못했다. 

$$
W = \{w \in \mathbb{R}^d | w_1^2 + w_2^2 + \cdots + w_d^2 = \alpha\}
$$

아래 그림은 학습 전 후로 고정된 unitary sphere 의 문서 워터마크와 임베딩 공간을 보여준다. 굳이 unitary sphere 를 쓰려는 이유는 normal distribution의 경우 그 값의 범위가 무한대이기 때문이다. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21926&authkey=%21AAstF537BgaczVA&width=688&height=300" width="688" height="300" />

## 실험 결과 

Unitary Sphere 에 대해서는 좀더 논의가 필요하다. 이 실험에서는 normal distribution 에서 std 를 제어함으로써 학습 성능의 변화를 살펴봤다. std 가 증가할수록 초기화된 임베딩 공간을 넘는 문서 워터마크가 존재하며, 학습의 불안정성이 커진다. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21930&authkey=%21ABxtBfbqotaWBtk&width=660" width="660" height="auto" />


## 결론 

임베딩 공간에 단어와 문서의 정보를 넣는 것은 둘 중 하나의 크기가 상대적으로 작을 때 가능하다. 
여기서는 단어 임베딩을 기준으로 문서 정보를 넣는 것을 고려하였다. 
반대로 문서 임베딩에 대해서 단어 임베딩을 넣는 것을 가정할 수도 있을 것으로 고려된다. 