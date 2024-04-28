---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2024-04-25
featured: true
img: 
title: 'Literature Reviews: Neural Memory'
description: 'This article is a review of several papers related to the neural memory'
---


| Date | Venue|  Title |
|------|------|------|
| 2017 | ICLR | Learning to Remember Rare Events
| 2023 | AAAI | Memory-Augmented Theory of Mind Network  
| 2019 | ICCV | Memorizing Normality to Detect Anomaly: Memory-augmented Deep Autoencoder (MemAE) for Unsupervised Anomaly Detection
| 2020 | ICLR | Generalization through memorization: Nearest neighbor language models





## Memory-Augmented Theory of Mind Network

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



* hierarchical attention to selectively retrieve information about others.
* rapid, selective querying of distal related past behaviours of others to deliberatively reason about their current mental state, beliefs and future behaviours
* learn the memory mechanisms to build the computational ToM capability into artificial social agents.
* ToMMY (Theory of Mind with MemorY)


* Learning memory-augmented neural networks is a powerful technique for multi-step reasoning (Sukhbaatar et al. 2015; Graves et al. 2016), handling rare events (Kaiser et al. 2017), meta-learning (Santoro et al. 2016) and rapid reinforcement learning (Le et al. 2021). 
* false-belief test to evaluate the ToM ability under high reasoning demand. False-belief tasks determine if the actor is maintaining an outdated belief about something that no longer
holds. A classic example is the Sally-Anne Test (Wimmer and Perner 1983; Baron-Cohen, Leslie, and Frith 1985), in which Anne secretly moves a toy out of the original box, causing Sally to falsely believe that the toy is still there.  A
version of the Sally-Anne Test for testing artificial theory of
mind agents introduced in (Rabinowitz et al. 2018; Nguyen
and Gonzalez 2021) take the form of grid-worlds. **When the actor tries to achieve the sub-task before reaching a goal, the position of the goal will be changed, a so-called swap event**. This event induces a false belief in the actor, and the observer needs to take the actor’s perspective to understand the actor’s false belief.


## Learning to Remember Rare Events

Rare event를 사용하고 확인하는 것은 쉽지 않다. 특히나 neural network는 long-range나 긴 데이터에 대해서 사용하기 쉽지 않다. 

메모리를 업데이트 하는 부분에 대해서 추가적으로 공부해야 한다. 


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
\hat{w} = h(w_i;\lambda) = \begin{cases} w_i & \text{if} w_i > \lambda \\ 0 & \text{othderwise} \end{cases}
$$

이 식은 ReLU에 의해서 다시 쓰여질 수 있다. 
$$
\hat{w} = \frac{\text{max}(w_i - \lambda, 0 ) * w_i}{|w_i - \lambda| + \epsilon}
$$

이 방식은 명시적으로 메모리의 엔트리를 선택하거나, 선택하지 않는 방식을 제공한다. 
weight 에 대한 magnitude를 감소시키기 보다 entropy를 감소시키는 방향으로 진행도있다. 
