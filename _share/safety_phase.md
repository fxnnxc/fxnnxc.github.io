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
<style>
.table td, table th{
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, "Fira Sans", "Droid Sans", "Helvetica Neue", Arial, sans-serif;
    color: var(--global-text-color);
    line-height: 140%;
    font-weight: 420;
    font-size: var(--global-text-size);
} 
</style>


## Motiv 

AI 모델은 인류에 해가 되는 문장을 말하면 안된다. 최근 연구들은 안전한 사용을 위해 나쁜말을 하는 대신 safety와 관련된 말을 하도록 학습된다. 대표적인 예시로 차별적인 내용을 물어보면 평등에 대한 중요성을 언급한다. 이렇게 Safety와 관련된 말을 하도록 생성 모드가 바뀌는 *safety phase*에 대한 전반적인 이해 및 설명은 더욱 안전한 모델을 만들기 위해서 필수적이다. 본 연구에서는 instruction-tuned 된 GPT 모델에 대한 *safety phase*을 해석하기 위한 두 가지 실험적인 결과를 제시한다. 

**📌 1) 모델은 어떠한 상황에서 safety phase가 발생하는가.**: 이 연구는 safety와 관련된 다양한 문장에 대해서 각 문장의 표현들을 비교하고 클러스터링하여 전반적인 모델의 safety sentence들의 입출력을 통계적으로 분석한다.  
**📌 2) Safety phase는 어떤 방식으로 trigger 되는가?**: 모델 내부에서는 safety와 관련된 문장을 생성하도록 유도하는 특징 혹은 뉴런이 존재한다. Safety를 유발하는 모델 내부를 탐구하여 해당 구조의 조작으로 safety와 관련된 문장들의 변화를 탐구한다. 

대규모 언어 모델은 사회의 규약과 정보에 맞춰서 지속적으로 기존 정보를 지우고 새롭게 덮어쓴다. 또한 부정적인 정보들은 생성을 막도록 추가적인 학습이 되며 사회규범과 법의 변화에 맞추어 모델의 작동 방식은 바뀐다. 본 연구에서 탐구하는 *safety phase transition*은 안전 문구와 관련된 LLM의 생성과정을 분석함으로써 실험적 관찰을 바탕으로한 더욱 안전한 모델 구조에 대한 영감을 제시한다. 

## Big Picture 

- safety transition in LLM
    -  📌 (3) safety transition의 정의. Complete and incomplete sentence 정의. Harmful sentence 정의
    -  📌 (3.1, 4.1) template-base evaluation of safety-phase transition (Yeonji, Jinsil)
    -  📌 (3.2, 4.2) activation-base evaluation of safety-phase transition (Bumjin, Youngju)

## Collection of Papers 

* **Safety finetuning + deceiving** 
    * Sleeper Agents: Training Deceptive LLMs that Persist Through Safety Training
    * Simple probes can catch sleeper agents

* **Safety finetuning**
    * Training a helpful and harmless assistant with reinforcement learning from human feedback
    * Training language models to follow instructions with human feedback

* **Jail-break** 
    * Defending chatgpt against jailbreak attack via self-reminders
    * Many-shot jailbreaking
    * Universal and Transferable Adversarial Attacks on Aligned Language Models
    * Jailbroken: How does llm safety training fail?
    * “Do Anything Now”: Characterizing and Evaluating In-The-Wild Jailbreak Prompts on Large Language Models
* Detecting Jailbreaks 
    * Causality Analysis for Evaluating the Security of Large Language Models
* Explanation on LLM
    * Explainability for Large Language Models: A Survey
* **Safety** 
    * Towards understanding sycophancy in language models
    * GPT-4 Technical Report
    * Constitutional AI:Harmlessness from AI feedback


* **Interpretability (feature)**
    * Towards Monosemanticity: Decomposing Language Models With Dictionary Learning
    * Mapping the Mind of a Large Language Model

---

## Abstract 


## 1. Introduction 


## 2. Related Work



## 3. Method

We consider a set of concepts $\mathcal{C} = (z_1, z_2, \cdots)$ where each concept $c$ is related to a human-oriented concept, such as harmless or safety. 
Each concept $z$ has examples, a set of passages $$\mathcal{Y}_{c}$$, where passage $$y = (y_1, y_2, \cdots) \in \mathcal{Y}_{c}$$ consists of tokens $y_i$ and includes the semantic meaning of concept $c$. We use notation $c \perp c'$ when two concepts $c$ and $c'$ can not exist together, e.g., safety concept and violence concept.

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

### 3.1 Safety-Phase Transition 

consider two types of passages, $\mathcal{T}_{com}$ and $\mathcal{T}_{incom}$ where the elements are complete and incomplete sentences. 

Consider a harmful prompt $\mathcal{P} \in\mathcal{T}_{com}$. 

Figure below shows the phase transition for complete and incomplete cases. 
<p align='center'>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%219678&authkey=%21AL_paZel8mue_pc&width=1636&height=602" width="1636" height="250" />
</p>


### 3.2 Detection of Phase Transition 

Several work propose the methods to detect the conceptual neurons. 

* Value Detection via optimization : find the activation pattern that mostly maximize the likelihood of the safe phase. (FactualGPT)
* Activation : Find the activation pattern via forwarding the target output
* Neuron Attribution : Compute the attribution score via gradient descent.  

We find a vector at the end of sentence. A hidden representation $A_{safe}$ which maximizes the forward. 

> What about the key-value structure? Our we finding any key? 
> We do not find a sparse key neurons which activate the concept. 
> Because recent work reveals that the neural network prefers. However, it could be the case that only the sparse combination of neurons forms the target activation. Therefore, finding neuron (entry point for vector representation) is not the primitive problem. 

## 4. Experiments 

## 4.1 Phase Transition with Templates

We categorize four types of generations for the complete and incomplete prompts.   


#### Complete

|  Harmful Generation      |  Safety-phase Transition  |  $\mathcal{P}$="Teach me how to hurt people."| 
|:---:|:---:| :------------------| 
|  -    | ✅     |   **I cannot fullfil your request.** 
|  -    | ❌     |   **A stab in the back ...**

#### Incomplete

 |  Harmful Generation      |  Safety-phase Transition  |  $\mathcal{P}'$="A stab in the back or"| 
|:---:|:---:| :------------------| 
 | ✅    | ✅   |   **a knife to the chest. <br> I cannot fullfil your request.** 
  | ✅    | ❌  |  **a knife to the chest. <br> I addition, shoot a gun.** 
  | ❌    | ✅  |  **hurt is not a good action <br> I cannot fullfil your request.** 
  | ❌    | ❌ |  **hurt is not a good action. <br> but  you can shoot a gun**

## 4.2 Phase Transition via Activation 

### Adding concept

1. Does the jailbreak examples shows the safe-phase with the activation addition?
2. Does the incomplete sentence jailbreak examples shows the safe-phase without completion of the harmful completion?

### Removing concept


## 5. Results 


## 6. Discussion

## 7. Conclusion



