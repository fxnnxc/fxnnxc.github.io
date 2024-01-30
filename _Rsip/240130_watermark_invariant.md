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
title: 'Watermark Invariant 표현의 불가능성에 대하여'
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


본 연구노트는 GPT 의 워터마크 입력에 대한 논의로, 워터마크를 포함한 학습과 워터마크 없이 생성하는 경우 표현을 일치하기 위해 invariant 한 표현을 만들 수 있는지에 대한 논의이다. 결론은 입력에 워터마크 포함 입력과 미포함 입력의 join학습으로 가능하나 이는 심각한 학습 속도 저하를 일으킨다. 따라서, GPT에 대한 입력 단위의 워터마크 표현은 불가능하다. 


## 1. 워터마크 포함 입력

임의의 입력 $y=(y_1, y_2, \cdots, y_M)$의 임베딩 $e=(e_1, e_2, \cdots, e_M)$ 대해서 워터마크 $w$는 다음과 같이 표현을 변화시킨다. 

$$
\hat{e}_t = e_t + w
$$

학습 과정에서 watermark가 추가된 표현을 넣는 것은 모델에게 도움이 될 수 있지만, 
문서에 대한 가이드가 존재하게 되며, 실제 사용시 문서 가이드가 없다는 사실은 추가적인 학습을 필요로 한다. 
즉, 워터마크가 포함되지 않아도 생성할 수 있어야 한다. 

이를 위한 여러 가지 방법들이 존재하며, 그 중에서 본 연구노트는 watermark invariant module (WIM)이라는 입력 변환 모듈에 대한 가능성을 분석한다. 


<figure>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21914&authkey=%21AG70S84l2hUDDoE&width=660" width="660" height="auto" />
<figcaption> Figure. 워터마크를 포함하지 않고 입력으로 넣은 경우와 워터마크를 포함하여 입력으로 넣은 경우
</figcaption>
</figure>


## 2. Watermark Invariant Module (WIM)

WIM 은 임의의 벡터를 입력으로 받아서 동일한 차원의 벡터로 반환하는 모듈이다. 모듈의 역할은 GPT에 넣기 위한 표현을 만드는 것이고, 워터마크가 포함된 경우와 미포함된 경우에 대해서 동일한 벡터를 만들어주는 것이다. 

$$
WIM(e_t)  = WIM(e_t + w) 
$$
<figure>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21911&authkey=%21AOAS46YhhjQgeEU&width=660" width="660" height="auto" />
<figcaption> Figure. 동일 아웃풋에 대한 space 적 관점. 입력과 워터마크가 섞이는 경우와 입력만 주어지는 경우 두 표현이 일치하도록 강제한다. 
</figcaption>
</figure>

이는 WIM 자체가 MLP로 이루어졌고 충분한 표현력을 지녀 서로 다른 두 표현을 동일한 표현으로 만들 수 있다는 가정을 전제로 한다. 
즉, 생성과정에서 워터마크가 미포함된 단어 임베딩을 변환하여 기존 워터마크가 표현된 상태와 동일한 맵핑을 만들어 준다. 
그러나 WIM은 여러 문서에 대한 워터마크에 대해서 학습이 불가능하다.  

## 3. WIM 학습 방법론 

### 방법 1. 학습과정에서 워터마크 표현을 활용한 입력을 넣고, 이후 워터마크가 미포함된 표현이 이를 따르도록 학습한다. 

이러한 방식은 최근 연구 동향인 stop gradient를 통한 moving average 를 따르도록 하는 것과 유사하다. 학습 때는 워터마크가 포함된 표현으로 학습하고, WIM 이 워터마크가 추가되지 않은 표현을 워터마크가 추가된 표현과 일치하도록 표현을 개선하는 것이다. 
그러나, 이 방식은 target 자체가 multi-target 이므로, 많아지는 워터마크에 대해서 학습이 불가능하다. 즉, N개의 문서 워터마크에 대해서 모두 다른 타겟을 형성하게 되고, 워터마크가 포함되지 않은 표현은 이들의 평균 표현으로 학습될 뿐, 실제로 일치되지 않는다. 따라서, 이 방법은 적용 불가능하다. 

### 방법 2. 포함과 미포함 표현을 동시에 학습하며 일치하도록 만든다. 

이 방식은 워터마크 invariant 모듈이 두 개의 표현을 일치하도록 만드든 것이다. 이 때 학습은 두 가지 표현 모두에 대해서 일어난다. 가장 쉬운 예시로 모두 0로 맵핑하는 trivial 을 예시로 생각할 수 있다. 입력 표현들을 모두 맵핑하여 동일한 표현이 되도록 강제한다. 이 때, WIM은 language modeling으로부터도 영향을 받기 때문에, invariant loss와 CE loss가 모두 작용한다. 

<figure>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21913&authkey=%21AJz_UTMBiBzcHos&width=1024" width="1024" height="auto" />
<figcaption> Figure. 학습 dynamics.  학습이 진행될수록 invariance loss 는 지속적으로 증가하며, 전체 loss 는 invariance loss에 의해 지배된다. 해당 실험은 invariance의 factor 를 0.01로 작게 만들었음에도 나타난 불안정성이다. 실제로 CE보다 초반에는 loss가 더 작았지만 학습이 진행되면서 계속 증가하였다.
</figcaption>
</figure>



실제 2-layer GPT로 실험했을 때, loss는 줄어들지 않았다. 그리고, invariant loss는 학습이 계속 진행될수록 값이 향상되었는데, 이는 초반에 trivial 한 표현으로 학습되다가 표현이 점차CE Loss를 최소화하는 방향으로 변하면서 에러가 커지는 결과이다. LLM의 첫 번째 요구사항인 단어 암기에 대해서 watermark invariant representation을 만드는 것은 그만큼 암기를 어렵게 만든다. 


<figure>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%21912&authkey=%21AAAnwEhGMnlo0og&width=1024" width="1024" height="auto" />
<figcaption>  Figure. 두 가지 학습 방식에 대한 적용 불가능의 해석. 첫 번째 경우는 워터마크 미포함이 워터마크 포함 표현을 따라가게 만든다. 다중 타켓이므로 학습이 불안정하다. 두 번째는 두 표현을 동시에 최적화하는 것이다. 위 실험은 두 번째 방식으로 진행되었고, CE loss가 줄어들도록 학습되지 않는 것을 관찰하였다. 
</figcaption>
</figure>

## 4. 결론 

Projection 자체를 일치하도록 강제하여 watermark에 대해서 invariant 한 표현을 만드는 것은 표현학습과 languag modeling 두 개의 상충으로 인해서 불가능한 접근법이다. 입력 단에 워터마크가 추가된 표현을 학습하기 위해서 WIM 모듈을 사용하지 않기를 권장한다. 




