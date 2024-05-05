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
| 2023 | NeurIPS | Neural Priming for Sample-Efficient Adaptation 
| 2023 | NeurIPS | Monitor-Guided Decoding of Code LMs with Static Analysis of Repository Context
| 2024 | Arxiv   | TransformerFAM: Feedback attention is working memory
| 2024 | Arxiv   | Leave No Context Behind: Efficient Infinite Context Transformers with Infini-attention

## TransformerFAM: Feedback attention is working memory


## Leave No Context Behind: Efficient Infinite Context Transformers with Infini-attention

## 9. Learning to Remember Rare Events

Rare event를 사용하고 확인하는 것은 쉽지 않다. 특히나 neural network는 long-range나 긴 데이터에 대해서 사용하기 쉽지 않다. 메모리를 업데이트 하는 부분에 대해서 추가적으로 공부해야 한다. 

## 10.  Memorizing Normality to Detect Anomaly: Memory-augmented Deep Autoencoder (MemAE) for Unsupervised Anomaly Detection

## Monitor-Guided Decoding of Code LMs with Static Analysis of Repository Context

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



## Learning to Remember Rare Events

Rare event를 사용하고 확인하는 것은 쉽지 않다. 특히나 neural network는 long-range나 긴 데이터에 대해서 사용하기 쉽지 않다. 메모리를 업데이트 하는 부분에 대해서 추가적으로 공부해야 한다. 


## Generalization Through Memorization: Nearest Neighbor Language Models 

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


## Memorizing Normality to Detect Anomaly: Memory-augmented Deep Autoencoder (MemAE) for Unsupervised Anomaly Detection


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
