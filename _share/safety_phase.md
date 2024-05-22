---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
date: 2024-05-21
giscus_comments: true
title: 'Explanation on Safety Phase Transition in Large Language Models'
description: '안전장치로 학습된 모델의 phase transition에 대한 체계적 연구. ' 
---


## Motiv 

AI 모델은 인류에 해가 되는 문장을 말하면 안된다. 최근 연구들은 안전한 사용을 위해 나쁜말을 하는 대신 safety와 관련된 말을 하도록 학습된다. 대표적인 예시로 차별적인 내용을 물어보면 평등에 대한 중요성을 언급한다. 이렇게 Safety와 관련된 말을 하도록 생성 모드가 바뀌는 *safety phase*에 대한 전반적인 이해 및 설명은 더욱 안전한 모델을 만들기 위해서 필수적이다. 본 연구에서는 instruction-tuned 된 GPT 모델에 대한 *safety phase*을 해석하기 위한 두 가지 실험적인 결과를 제시한다. 

**📌 1) 모델은 어떠한 상황에서 safety phase가 발생하는가.**: 이 연구는 safety와 관련된 다양한 문장에 대해서 각 문장의 표현들을 비교하고 클러스터링하여 전반적인 모델의 safety sentence들의 입출력을 통계적으로 분석한다.  
**📌 2) Safety phase는 어떤 방식으로 trigger 되는가?**: 모델 내부에서는 safety와 관련된 문장을 생성하도록 유도하는 특징 혹은 뉴런이 존재한다. Safety를 유발하는 모델 내부를 탐구하여 해당 구조의 조작으로 safety와 관련된 문장들의 변화를 탐구한다. 

대규모 언어 모델은 사회의 규약과 정보에 맞춰서 지속적으로 기존 정보를 지우고 새롭게 덮어쓴다. 또한 부정적인 정보들은 생성을 막도록 추가적인 학습이 되며 사회규범과 법의 변화에 맞추어 모델의 작동 방식은 바뀐다. 본 연구에서 탐구하는 *safety phase transition*은 안전 문구와 관련된 LLM의 생성과정을 분석함으로써 실험적 관찰을 바탕으로한 더욱 안전한 모델 구조에 대한 영감을 제시한다. 

## Collection of Papers 

* **Safety finetuning + deceiving** 
    * Sleeper Agents: Training Deceptive LLMs that Persist Through Safety Training
    * Simple probes can catch sleeper agents

* **Safety finetuning**
    * Training a helpful and harmless assistant with reinforcement learning from human feedback
    * Training language models to follow instructions with human feedback

* **Jail-break** 
    * Defending chatgpt against jailbreak attack via self-reminders
    * Many-shot Jailbreaking

* **Safety** 
    * Towards understanding sycophancy in language models

* **Interpretability (feature)**
    * Towards Monosemanticity: Decomposing Language Models With Dictionary Learning
    * Mapping the Mind of a Large Language Model

---

## Abstract 


## Introduction 



## Related Work



## Method

We consider a set of concepts $\mathcal{C} = (z_1, z_2, \cdots)$ where each concept $c$ is related to a human-oriented concepts, such as harmless and safety. 
Each concept $z$ has examples as a set of passages $$\mathcal{Y}_{c}$$, where passage $$y = (y_1, y_2, \cdots) \in \mathcal{Y}_{c}$$ consists of tokens $y_i$ and includes the semantic meaning of concept $c$. We use a notation $c \perp c'$ when two concepts $c$ and $c'$ can not exist together, e.g., safety concept and violence concept.

We define two terms *phase* and *phase transition$ as follows. 

> Definition: **Phase** <br>
>> A $c$-*phase* is a subset of time steps $[t_{begin}, t_{end}]$ where the generated content $$({y}_{t_{begin}}, \cdots, {y}_{t_{end}})$$ with $${y}_{t_{k}} \sim P_{\theta}(\cdot \vert y_1, \cdots, y_{t_{k}-1})$$ can be included in the set of concept-related passages $$\mathcal{Y}_{c}$$.

The definition of *phase* assumes a passage $y$. The inclusion of an element in the set element in $\mathcal{Y}_{c}$ can be either determined by a human-annotator or text similarity scores with the examples in the set.  

<!-- We define *safe phase* which generates safety related passes $\mathcal{Y}_{safety}$   -->

> Definition: **Phase Emergence**   <br>
>> A *phase emergence* is the time step when the generation includes new concept $c'$ as a phase. 

> Definition: **Phase Transition**   <br>
>> A *phase transition* is a $c$-phase emergence with the disappearance of existing concept $c'$ such that $ c' \perp c$.  


Rigorously, the definition of phase transition 

## Experiments 


## Results 


## Conclusion



