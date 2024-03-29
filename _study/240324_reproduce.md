---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-03-17
featured: true
title: 'Towards Knowledge Memories in Transformers'
description: '트랜스포머에 추가적인 입력을 같이 넣어서 생성물을 컨트롤 하는 연구'
---


## Neural Memories in Transformers 





## Task Definition 

Given a transformer architecture and external memories. 
We link the internal representation of transformer to the external memories to fetch task information and adapt language generation. 
We have two types of models

* Pretrained Language Model
* Knowledge 

Our goal is to construct the representation space so that the adapted LLM utilizes the memory and has higher performance where the task-knowledge is required. 

## Memory Tuning Setting 

We train an additional memory to memorize the contents So that the knowledge is stored in the model parameters. 
To do this, we need three data types in total

* query : questions that LLM must answer well. 
* answer : predicted answer
* context : provided context information for the query 
* external_context : not provided in the context information for query 

The context information 

### GPT

We inject memory module in the GPT layers to tune the model. In the knowledge memorization phase, the parameters are either frozen or trained. 




### Text-to-Text Memories 

The hidden representation of encoder is adapted to the memory-based representation. 


|model        | info |     EM    |       F1      | checkpoint     |
|-------------|------|-----------|---------------|----------------|
| T5          | small  | 75.39   |  83.68        | marry_go_round |
| T5          | base   | 81.41   |  88.80        | smell_of_sense |
| T5          | large  | 84.28   |  90.76        | red_pool       |
| T5          | 3B     | 86.78   |  92.80        | bad_man        |
| T5 v1_1     | small  |
| T5 v1_1     | base   |
| T5 v1_1     | large  |
| T5 v1_1     | 3B     |
| T5 FLAN     | small  |
| T5 FLAN     | base   |
| T5 FLAN     | large  |
| T5 FLAN     | 3B     |


---

## Notes 

* T5.1.1의 fineutne 속도가 더 느린 것 같다. 