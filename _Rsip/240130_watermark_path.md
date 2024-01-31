---
layout: share-distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2024-01-29
featured: true
img: 
title: 'Watermark Points and Paths'
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


## 문서 워터마크 

## Watermark Points and Paths

문서에 대한 워터마크를 가정하는 경우, 워터마크 생성은 두 가지로 나뉠 수 있다. 

1. 문서에 대해서 워터마크를 배정한다. 
2. 문서 내 컨텐츠에 대해서 워터마크를 배정한다. 

<figure>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21915&authkey=%21AFYvizi8glfkmr8&width=1024" width="1024" height="auto" />
<figcaption> Figure. 문서의 워터마크 두 가지 방식. (중간) 2D point 워터마크 가정. (오른쪽) 2D path 형태의 워터마크 가정. 
</figcaption>
</figure>

문서 워터마크의 기본 방식은 문서 하나에 대해서 하나의 워터마크를 가지는 것이다. 워터마크를 하나의 벡터로 표현 한다면, 이는 point에 해당하며, LLM의 입력에 대해서 해당 문서의 point를 넣어서 학습하는 게 된다. 이 방식의 장점은 문서를 구분하는데 유용하다는 점이고, 단점은 문서내 컨텐츠에 대한 모델링이 불가능하다는 점이다. 즉, 짧은 문서든 긴 문서든 모두 동일한 point를 가지게 된다. 

문서 워터마크는 point 대신에 path로 주어질 수 있다. 
만일 가능한 워터마크가 무수히 많다면, 2번 안은 단순히 원천 문서를 추정하는 것을 넘어서 문서 내 컨텐츠의 위치까지 추정하는 게 된다. 
이를 위해서는 워터마크에 대한 일종의 path를 생성해야 한다. 


## Iterative Training 

이전 논의에서 보았듯이 워터마크를 넣으면서 학습하는 경우, 워터마크 없이 생성은 불가능하며 이를 위해서 watermark invariance module 을 설명하였다. 이에 대한 대안으로 입력 단에 표현을 맞추는 대신, 다른 입력이여도 동일한 아웃풋을 강제하는 방식으로 모델을 학습시킬 수 있다. 

<figure>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21916&authkey=%21AJ2u8HYogEfVH1A&width=1024" width="1024" height="auto" />
<figcaption> Algorithm. 다른 입력에 대해서 아웃풋을 동일하게 강제한다. 여기서 아웃풋은 생성된 단어이다. 이 방식은 WIM 의 경우와 다르게 추가적인 loss가 필요하지 않다. 일종의 regularization으로 볼 수 있다. 
</figcaption>
</figure>


## 실험 결과 

실험 결과 watermark 의 타입에 따라서 학습 결과가 다르지 않았다. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21938&authkey=%21AFRB9oGa3CnicWM&width=805&height=594" width="805" height="594" />

이 결과에 대해서 두 가지 설명 방식이 있다.

1. watermark 타입에 따라서 큰 차이가 없다. 
2. watermark prediction 모델은 3레이어 모델이다. 따라서 충분한 표현 공간을 가지고 있다. 

## 결론 

본 실험으로 watermark prediction을 진행하면서 language modeling 이 가능한 것을 확인하였다. 또한 watermark loss alpha 에 따라서 CE Loss가 크게 변하지 않았던 것은 두 개의 Object를 동시에 만족하면서 학습하는 게 가능할 것으로 판단된다. 