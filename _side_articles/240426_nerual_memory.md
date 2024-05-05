---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2024-04-30
featured: true
img: 
title: 'Literature Reviews: Neural Memory, kNN augmentation and some other recent trends.'
description: 'This article is a review of several papers related to the neural memory and utilization of data embeddings (12 papers)'
---

## Reading List

| Date | Venue|  Title | 
|------|------|------|
| 2023 | NeurIPS | Neural Priming for Sample-Efficient Adaptation 
| 2023 | NeurIPS | Monitor-Guided Decoding of Code LMs with Static Analysis of Repository Context
| 2023 | NeurIPS | ResMem: Learn what you can and memorize the rest
| 2023 | NeurIPS | Accessing Higher Dimensions for UnsupervisedWord Translation
| 2023 | NeurIPS | Lift Yourself Up: Retrieval-augmented Text Generation with Self-Memory
| 2023 | NeurIPS | Exposing Attention Glitches with Flip-Flop Language Modeling
| 2024 | Arxiv   | TransformerFAM: Feedback attention is working memory
| 2024 | Arxiv   | Leave No Context Behind: Efficient Infinite Context Transformers with Infini-attention
| 2017 | ICLR | Learning to Remember Rare Events
| 2019 | ICCV | Memorizing Normality to Detect Anomaly: Memory-augmented Deep Autoencoder (MemAE) for Unsupervised Anomaly Detection
| 2020 | ICLR | Generalization through memorization: Nearest neighbor language models
| 2023 | AAAI | Memory-Augmented Theory of Mind Network  


## 위 논문들에 대한 전반적인 생각 및 앞으로 방향성 

1. 모델에 데이터의 표현을 모두 저장하는 것보다 데이터에 표현을 외부에 두고 활용하는 방식이 대두되고 있음. <br> (ResMem, MemAE, Generalization through memorization, Memory-Augmented Theory of Mind Network ) 
2. Transformer에 augmentation하는 방식은 독립적인 logit에 결합하는 방식으로 진행됨 (Monitor, kNN 결합등)
3. 트랜스포머 계열에서 Attention의 믿음이 약해지고 RNN 계열이 강해지고 있음. (Attention Glitch, TransformerFAM, Infini-attention)

* 내가 연구한 것도 문서에 대한 정보를 저장하기 위해서 마지막 레이어를 교체 하였는데, 이는 최근 연구되는 마지막 레이어의 표현을 변경하는 것과 유사하다. 
* 마지막 레이어에만 표현을 augmentation 하는 부분은 logit에 가까운 부분만 수정하기 때문에 중간 레이어를 바꿔야 좀더 많은 것들을 바꿀 수 있다. 그러나 제대로 알지도 못하는 중간 레이어 값을 수정하는 것은 설명/해석의 결함을 지닐 가능성이 높다. 
* 트랜스포머에서 RNN 계열을 도입하는 경우, 더욱 효율적으로 long-term 예측을 진행할 수 있다. 결국 "attention is all you need"로 많은 부분이 바꼈지만, 슬슬 한계를 보이고 있으며, convolution (Mamba) 혹은 RNN (TransformerFAM)과 같은 연구들이 진행되는 것이다. 이러한 모델링을 결합하면 결함이 없는 모델링이 가능할 것으로 보인다. Memorizing Transformer, ReadAgent 와 같은 연구들도 long-context를 해결하기 위해서 연구되었다. 마찬가지로 위 연구들도 soft-attention이 지니는 너무 soft함을 해결하려고 노력하는 것이다. 다음 Transformer 구조는 RNN을 결합한 모델링이 될 것이라고 믿어 의심치 않는다. 

-----

## 1. Neural Priming for Sample-Efficient Adaptation 

주요 메소드 
1. 특정 레이블을 가지고 있는 데이터를 묶어서 클러스터를 형성한다. 
2. 예측할 때, 해당 클러스터로 형성된 임베딩 벡터를 활용한다. 


이 연구는 Pretraining 데이터를 기반으로 한 유사한 이미지 설명을 가져와 예측에 활용하는 Neural Priming Pool을 제안하였다. 
Pretraining 시 사용된 학습 데이터는 후속 예측에 임베딩을 제공하는 데 사용한다. 
이러한 방법은 기계 번역에서 사용되는 kNN 검색 기반 예측에 Pretraining 데이터를 활용하는 것이다. 

이는 모델이 데이터에 대한 학습으로 인한 편향을 활용하는 데 관련이 있어보인다. 

(이전에 기영님이 모델이 가정한 내용과 실제 레이블 간의 입력 기여가 다르다는 것처럼, Pretraining 데이터의 임베딩 활용과 관련이 있음.)

## 2. Monitor-Guided Decoding of Code LMs with Static Analysis of Repository Context


해당 방식은 코드 생성 언어 모델(Code generation LLM)에서 마지막 로짓을 가리는 마스크를 기반으로 생성을 안내하는 방식이다. 
이는 워터마크에서 다음 단어에 대한 분포를 수정하여 추후 예측에 사용한 것처럼 해당 연구도 유효한 코드를 생성하기 위해 샘플링하는 단어를 제약하는 것이다.

## 3. ResMem: Learn what you can and memorize the rest

* 해당 방식은 LLM에서 KNN으로 성능을 올리는 부분이 비슷하다. 
* 차이점은 prediction task의 단일 모델에 대해서 residual을 모두 암기해놨다는 점이다. 또한 이론적으로 risk에 대한 bound를 계산할 수 있다. 

Base 모델을 학습한 후에는 추가적인 모듈을 Residual에 암기시킨다. 
그런 다음 예측할 때 kNN을 사용하여 추가적인 샘플을 찾아 Residual 부분을 보완한다.

## 4. Accessing Higher Dimensions for UnsupervisedWord Translation

Cooccurrence로 벡터를 만들어서 효율적인 translation을 가능하게 만들었다. (Coocmap) 

## 5. Lift Yourself Up: Retrieval-augmented Text Generation with Self-Memory

Interactive한 self-play 방식의 메모리 사용을 제안함. 

* primal problem: better memory prompts better generation (메모리 유사도가 높을수록 BLEU score가 높았음)
* dual problem: better generation also prompts better memory (fixed memory대신에 필요한 메모리를 생성하면 더 좋은 메모리를 만들 수 있음.)

더 좋은 메모리를 뽑도록 학습한다. (Target distribution에 대해서 KL divergence를 최소화)
더 좋은 메모리를 생성하도록 학습한다. 

## 6. Exposing Attention Glitches with Flip-Flop Language Modeling


문제에 대한 이해가 부족한 것 같다. 
소스와 타겟이 주어졌을 때 각 단어의 공생(cooccurrence)을 측정하는 것은 이해됩니다. 선택된 단어들 간의 최대한 일치(matching)를 하는 것으로 보입니다.
* Vecmap과 차이: coocurrence는 데이터의 표현이 다른 단어들로 이루어진다. 단어와 같이 나오는 것으로 표현하였음. 


## 7. TransformerFAM: Feedback attention is working memory

메모라이제이션을 위한 토큰을 사용하여 long-range token processing을 가능하게 만들었다. 
값을 명시적으로 전달해주는 것 뿐만아니라 ㅈ높은 레이어에 있는 값을 아래로 내릴 수 있기 때문에 
Transformer기반 모델이 특정 대상을 더욱 오랫동안 생각하도록 만드는 것 같다. 

## 8. Leave No Context Behind: Efficient Infinite Context Transformers with Infini-attention

중간에 RNN기반의 모델을 사용하여 표현이 전달되도록 만들었다. 
요즘 트렌드는 트랜스포머 기반 모델이 recurrent한 특성을 지니는 hidden state를 만들도록 연구되는 것 같다. 


## 9. Learning to Remember Rare Events

Rare event를 사용하고 확인하는 것은 쉽지 않다. 특히나 neural network는 long-range나 긴 데이터에 대해서 사용하기 쉽지 않다. 메모리를 업데이트 하는 부분에 대해서 추가적으로 공부해야 한다. 

## 10.  Memorizing Normality to Detect Anomaly: Memory-augmented Deep Autoencoder (MemAE) for Unsupervised Anomaly Detection


해당 방식은 Code generation LLM에 대해서 마지막 Logit을 가릴 mask를 기반으로 generation을 guide 하는 방식이다 watermark에서 다음 단어에 대한 분포를 수정하여 추후 예측을 위해서 사용했던 것처럼 해당 연구도 Valid code를 생성하기 위해서 샘플하는 단어를 제약하는 것이다. 


## Neural Priming for Sample-Efficient Adaptation 

Pretraining 데이터에 대해서 유사한 이미지 description을 가져와서 예측에 활용하는 neural priming pool을 제안하였다. 
pretraining 당시 사용했던 학습 데이터는 이후 예측을 하는데 있어서 임베딩을 활용한다. 이러한 방식은 MT에서 활용하는 kNN search기반 예측에 대해서 
pretraining 데이터를 활용하는 방식이다. 이는 모델이 데이터에 대해서 학습하며 생긴 bias를 활용하는 것과도 연관되어 있다. (이전에 기영님이 모델이 생각하는 것과 실제 레이블에  대해서 input attribution이 다르다는 것처럼, pretraining 데이터에 대한 임베딩 활용)

1. 특정 레이블을 가지고 있는 데이터를 묶어서 클러스터를 형성한다. 
2. 예측할 때, 해당 클러스터로 형성된 임베딩 벡터를 활용한다. 


Autoencoder에 대해서 임베딩을 생성하고 key-value로부터 값을 선택하여 reconstruction하는 부분을 제안하였다. 
제안한 방식은 key에 대한 weight을 설정하는 부분에서 hard shrinking 을 사용하여 sparsity를 강제하였다. 

$$
\hat{w} = h(w_i;\lambda) = \begin{cases} w_i & \text{if} ~ w_i > \lambda \\ 0 & \text{othderwise} \end{cases}
$$

이 식은 ReLU에 의해서 다시 쓰여질 수 있다. 
$$
\hat{w} = \frac{\text{max}(w_i - \lambda, 0 ) * w_i}{|w_i - \lambda| + \epsilon}
$$

이 방식은 명시적으로 메모리의 엔트리를 선택하거나, 선택하지 않는 방식을 제공한다. 
weight 에 대한 magnitude를 감소시키기 보다 entropy를 감소시키는 방향으로 진행도있다. 



## 11. Generalization Through Memorization: Nearest Neighbor Language Models 

마지막 레이어에 대해서 그 표현 값을 KNN neighbor search하여 값을 가져오는 방식을 택하였다. 그리고 마지막 확률값을 LLM과 search 한 것을 섞었다. 
Wikitext에 대해서는 KNN search 한 부분을 더 사용하는 것이 ($\lambda =0.5$) 책에 대해서는 KNN 부분을 덜 쓰고 LLM의 아웃풋을 사용하는 것이 ($\lambda=0.2$)가 더 좋은 성능을 보였다. 이는 Wikitext의 경우 좀더 extractive 한 성질을 가지고 있기 때문이다. 

마지막 레이어에 대해서 그 표현 값을 KNN neighbor search하여 값을 가져오는 방식을 택하였다. 그리고 마지막 확률값을 LLM과 search 한 것을 섞었다. 
Wikitext에 대해서는 KNN search 한 부분을 더 사용하는 것이 ($\lambda =0.5$) 책에 대해서는 KNN 부분을 덜 쓰고 LLM의 아웃풋을 사용하는 것이 ($\lambda=0.2$)가 더 좋은 성능을 보였다. 이는 Wikitext의 경우 좀더 extractive 한 성질을 가지고 있기 때문이다. 

* *Representation* --> *Next Word로 맵핑*
$$
(\mathcal{K}, \mathcal{V}) = \{ (f(c_i), w_i) | (c_i, w_i) \in \mathcal{D} \}
$$

각 단어마다 확률을 계산해서 업데이트 해준다. 
$$
p_{KNN}(y|x) \propto \sum_{(k_i, v_i) \in \mathcal{N}} 1_{y=v_i} \exp{-d(k_i, f(x))}
$$


추가적으로KNN 으로부터 뽑은 지식에 대해서 LM의 표현과 linear interpolation을 하였다. 

$$
p(y|x) = \lambda p_{knn}(y|x) + (1-\lambda)p_{LM}(y|x)
$$

* fast nearest neighbor retrieval in high dimensional spaces. FAISS speeds up search by clustering the keys and looking up neighbors based on the cluster centroids, while reducing memory usage by storing compressed versions of the vectors.

## 12. Memory-Augmented Theory of Mind Network

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%212445&authkey=%21ANjtEw50WYZZvNg&width=660" width="660" height="auto" />

이 연구에서 메모리를 사용하는 방식은 trajectory에 대해서 과거 정보를 key-value로 저장하고 현재 상태에서 정보를 뽑아내는 부분을 구현하였다. 
1. 현재 trajectory가 주어지고, 현재 상태에서 trajectory에서 cosine 유사도 기반으로 M 개의 query를 생성한다.  ($q_m, m=1,2,\cdots, M$)
2. 과거 정보는 key-value memory의 형태로 저장되어 있고, 각 $q_m$마다 memory를 retrieval한다. 

$$
\bar{v}_m = \sum_{v_j^t \in \mathcal{M}.value} \operatorname{attn}(q_m, k_j^t) v_j^t
$$

3. 쿼리마다 생성된 value들을 다시 attention을 기반으로 섞는다. 

이러한 방식은 trajectory가 여러 state 들로 구성되어있기에 hierarchical하게 메모리를 뽑는 구조이다. 이 과정에서 attention으로 정보를 섞는 것은 두 번 나타난다. 
key-value를 생성하는 방식은 LSTM의 hidden state로부터 forward 함수를 이용해서 연산한 것이다. 



