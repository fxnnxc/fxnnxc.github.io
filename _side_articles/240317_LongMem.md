---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2024-03-17
featured: true
img: https://onedrive.live.com/embed?resid=AE042A624064F8CA%211272&authkey=%21ACjQzrD9FEbgMk4&width=1024
toc:
  - name: Introduction
    subsections:
      - name: Q&A
      - name: History
      - name: Contribution
  - name: Methods
    subsections:
      - name: Side Network
      - name: Cached Memory
      - name: Chunks
  - name: Experiments
    subsections:
      - name: Set Up
      - name: Long Context
      - name: Demonstrations
  - name: Notes

title: 'LongMem 논문 리뷰 (NeurIPS 2023)'
description: '이 포스팅은 Augmenting Language Models with Long-Term Memory 논문에 대한 공부입니다.'
---


이 글은 [Augmenting Language Models with Long-Term Memory
](https://proceedings.neurips.cc/paper_files/paper/2023/hash/ebd82705f44793b6f9ade5a669d0f0bf-Abstract-Conference.html) <d-cite key="wang2024augmenting"> </d-cite> 논문에 대한 리뷰입니다. 


<h2 id="introduction"> 1. Introduction </h2>


<h3 id="q-a"> Pre-Questions and Answers  </h3> 

1. **Long-term 메모리는 어떻게 표현되었는가?** : 캐시에 in-context를 저장하는 부분이 있으며, retrieval 을 위한 SideNet를 구현하였다. 메모리는 queue 형태로 오래된 메모리를 지운다. 메모리 사이즈가 충분히 크기 때문에 긴 정보를 저장할 수 있다. 
2. **Memory Retrieval Module의 입력과 출력은 무엇인가?** prediction을 위한 query가 주어지면, 메모리에서 key-value에 대한 attended value combination을 반환한다. side-network 는 해당 정보를 sigmoid function으로 섞는다.  
3. **기존 Pretrain된 LLM을 finetuning하는 형태인가**? 아니면 처음부터 학습하는 구조인가?  해당 연구는 pre-train된 상태를 활용하였고, GPT2모델 (407M)에 대해서 long-context를 학습하였다. 256 batch-size, 1024 sequence 길이를 활용하였다. 


<h3 id="history"> Historical Notes </h3>

논문에 언급되어 있는 이전 연구에 대한 주요 흐름과 이 논문에서 해결하는 문제는 다음과 같다. 

1. GPT3는 GPT2에서 가진 1k 토큰 개수를 2k로 늘렸다. 늘어나는 토큰 개수는 더 넓은 in-context를 다룰 수 있게 만들었지만, long-range에 대한 근본적인 해결책은 될 수 없다. 
2. 기존 MemTRM 모델은 non-differentiable 외부 메모리에 대한 retrieval 방식으로 효율성을 개선하였다. CPU에 메모리를 저장해두고 값을 가져와서 사용하는 방식이다.  
3. MemTRM에서는 memory staleness(메모리 진부)문제가 발생하였다. 이는 수많은 메모리가 모델이 업데이트 되는 과정에서 distributional shift와 같은 표현 차이로 비효율적인 표현공간이 형성되었다.  


<h3 id="contribution"> Main Contribution of Paper </h3>

논문에서는 두 가지 main task 가 있다. 
1. 책에 대한 모든 정보를 다루는 것 : PG22 데이터는 책에 대한 텍스트를 가지고 있다. 본 논문은 책의 내용을 모두 인지한 상태의 LLM을 목표로 한다. 
2. 수천개의 task-relevant demonstration examples을 활용하는 것. ICL (in-context learning)는 demonstration으로부터 주어진 문제를 효율적으로 풀 수 있다. 본 논문에서 제안하는 방식 수많은 in-context example 을 메모리에 저장할 수 있다. 


<h2 id="methods"> Methods  </h2> 

논문에서 제안한 방식을 이해하기 위해서는 3가지 모듈을 이해해야 한다. 

* Side Network
* Cached Memory Bank
* Tokens to a Chunk

<center>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211272&authkey=%21ACjQzrD9FEbgMk4&width=1024">
</center>


<h3 id="side-network"> Side Network  </h3> 


SideNetwork는 LLM의 residual representation과 이전 side network의 이전 레이어의 값을 섞는 방식으로 진행된다. 

$$
\mathbf{H}^{l}_{\operatorname{Side}} 
= f_{\Theta^{l}_{\operatorname{Side}}}
\mathbf{H}_{\operatorname{Side}}^{l-1} + (\mathbf{H}^{2l}_{\operatorname{LLM}}
- \mathbf{H}^{2l-2}_{\operatorname{LLM}})
$$

Side Network는 LLM의 파라미터를 그대로 가져와서 메모리의 key-value를 섞기 위한 모듈이다. 논문에서는 섞는 과정을 학습하기 위해서 LLM을 weight를 그대로 가져와서 SideNet을 초기화하였다.  

SideNet은 외부 메모리의 attention 연산으로부터 value를 가져온다. 기존 LLM의 in-context 정보와 메모리의 정보를 활용하기 위해서 <i> joint attention mechanism </i> 을 활용하였다. 

$$
\begin{gather}
\mathbf{H}^l
 = \operatorname{sigmoid}(g) \cdot \mathbf{A} 
 + (1-\operatorname{sigmoid}(g)) \cdot \mathbf{M}  \\
 \mathbf{A} = \operatorname{softmax} (\frac{\mathbf{Q}\mathbf{K}^\top}{\sqrt{d}}) \mathbf{V}, 
 \mathbf{M} = \operatorname{Concat} \{ \operatorname{softmax}
 (\frac{\mathbf{Q}_i \tilde{\mathbf{K}}_i^{\top}}{\sqrt{d}}) \tilde{\mathbf{V}}_i 
 \}_{i=1}^{\vert x \vert}
 \end{gather}
$$

이 때 $\sim$이 붙은 key와 value는 메모리로부터 얻은 정보이다. $g$ 는 각 head별로 정의되는 학습하는 파라미터이다. 

<h3 id="cached-memory"> Cached Memory Bank  </h3> 

메모리는 $M$ 개수 key-value를 CPU에 저장하는 모듈이다. GPT에서 연산할 때, 발생하는 key, value 표현들을 메모리에 queue 형태로 넣는다. 논문에서 사용한 사이즈는 [8K, 16K, 32K, 65K]이다. 해당 메모리는 context정보를 저장하기 위해서 사용되므로, GPT의 내부 파라미터는 아니다. 


<h3 id="chunks"> Token-to-Chunk  </h3> 

외부 메모리는 많은 key를 가지고 있으므로 모든 key에 대해서 연산하는 것은 비효율적이다. 논문에서는 이를 개선하기 위해서 토큰을 chunk로 쪼개 attention을 계산하였고, top-K 개의 값을 가져왔다. 

1. Chunk size $csz$를 정한다. (논문에서는 csz=4$)
2. 메모리 bank에 $M$개수의 key가 있다. 
3. 연속된 key를 chunk로 묶는다. ($M/csz$ 개수 메모리)
4. Attention을 계산하고 Top-$K/csz$ 개수의 키를 고른다. 
5. Chunk의 정보를 flatten 한다. ($K/csz * csz = K$ 개 메모리) 

<h2 id="experiments"> Experiments  </h2> 


<h3 id="set-up"> Set Up  </h3> 


* Backbone LLM:  $L′ = 24,H = 16, d = 64$
* SideNet:  $L = 12, H = 16, d = 64$

학습 세팅 
* 전체 토큰 수: 26B
* global batch size: 256
* sequence length: 1024
* retrieved key: 64
* csz: 4 
* memory-augmentation layer: 9-th layer of SideNet

데이터 배치를 형성하는 방법은 길이가 비슷한 애들을 묶어서 고정된 길이로 만들고, batch index에 다른 길이의 문서를 넣는 방식을 사용하였다. 

<center>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211276&authkey=%21AEnE6wW_nierJ6w&width=1024">
</center>


PG22에 대한 데이터 통계는 다음과 같다. 

<center>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211273&authkey=%21ALIwMbAi5o3Jb-w&width=1024
">
</center>


<h3 id="long-context"> Memorizing Long Context  </h3> 

메모리 사이즈를 고정하고 PG22데이터에 대한 Perplexity는 MemTRM과 TRIME 보다 더 낮은 것을 확인할 수 있다. 책의 정보를 암기하는데 있어서 이전 정보들을 같이 주는 것은 효율적이고, LONGMEM 방식의 외부 메모리 저장은 더 높은 성능을 보인 것이다. 

<center>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211277&authkey=%21ADMroYpiZWdfPW4&width=1024">
</center>


<h3 id="demonstrations"> Large Number of Demonstrations  </h3> 

Natural Language Understanding 문제에 대해서도 2000개의 demonstration을 주고 주어진 문제를 풀게 만들었다. GPT에 넣는 형태는 `di="Review: xi Sentiment: yi` 이다.

<center>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211275&authkey=%21AHZrLnUo8oZCXIs&width=1024">
</center>

메모리 사이즈가 반드시 크다고 좋은 것은 아니다. context의 사이즈가 작다면 적은 수의 메모리를 효율적으로 사용하는 것이 더 좋다. 아래 실험에서는 65K메모리에 대해서 상대적으로 적은 수의 메모리를 쓰는 경우 성능이 얼마나 향상되는지 확인하였다. 

<center>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211274&authkey=%21ADPebJJw0BsIFtM&width=1024">
</center>


<h2 id="notes"> Notes  </h2> 

주어진 context에 대해서 다음 단어를 예측하는 GPT의 쓰임이 늘어남에 따라서 더 길고 많은 정보들을 처리해야 한다. 메모리 기반으로 GPT의 내부 연산을 처리하는 방법은 더욱 정교하고 효율적인 계산을 위해서 필수적인 구조적 개선이다. NeurIPS 2022에 나온 memorizing transformer (MemTRM)과 NeurIPS 2023에 나온 이 연구처럼 앞으로도 더 많인 메모리 기반 모델링이 연구될 것 같다. 
