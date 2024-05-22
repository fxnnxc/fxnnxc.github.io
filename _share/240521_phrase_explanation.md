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

AI 모델은 인류에 해가 되는 문장에 대해서 말하지 않도록 추가적인 학습이 된다. 
안전한 사용을 위해 모델은 나쁜말을 하는 대신, 그러한 말을 하면 안되는 이유를 만들어 낸다. Safety와 관련된 말을 하도록 생성 모드가 바뀌는 safety phase에 대한 이해는 더욱 안전한 모델을 만들기 위해서 필수적이다. 본 연구에서는 instruction-tuned 된 GPT 모델에 대한 두 가지 실험적인 결과를 제시한다. 1) 모델은 어떠한 상황에서 phase가 발생하는가. 이 연구는 safety와 관련된 다양한 문장에 대해서 각 문장의 표현들을 비교하고 클러스터링하여 전반적인 모델의 입출력을 통계적으로 분석한다.  2) safety는 어떠한 방식으로 trigger가 되는가? 모델 내부에서는 safety와 관련된 문장을 생성하도록 유도하는 특정한 표현 혹은 뉴런이 존재한다. 이를 탐구하여 해당 뉴런의 조작으로 safety와 관련된 문장들이 어떠한 변화를 보이는지 탐구한다. 인공지능 모델은 지속적으로 학습하며 기존 정보를 지우고 새롭게 덮어쓴다. 또한 부정적인 정보들은 생성을 막도록 추가적인 학습이 된다. 본 연구에서 탐구하는 safety phase transition은 안전문구와 관련된 LLM의 생성과정을 분석함으로써 실험적 관찰을 바탕으로한 더욱 안전한 모델 구조에 대한 영감을 제시한다. 

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


## Experiments 


## Results 


## Conclusion



