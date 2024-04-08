---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-04-07
featured: true
title: 'Simset 방식은 document memorization으로 Factuality에 대해서 높은 성능을 낼 수 있는가? (Korean)'
description: '문서를 암기하도록 저장하는 것은 도움이 된다. 추가적인 궁금증은 구조적인 메모리로 문서를 암기하는 것이 도움이 되는지 여부이다.'
---
## Does Structural Document Memory Help Factual Recall? 

* 🗓️ **Experiment Date** : 24.04.08 
* 🌸 **Scope** : Few-shot classification for large language models with memorization adaptation. We use FEVER dataset 449 pages and related validation question <br> (#samples: valid 18999 train 311431). 
* 🧑🏻‍💻 **Code**:[TBW]()
* 😮 **Status** : Running Experiments 
  - [ ] &nbsp; *Documentation of Settings*
  - [ ] &nbsp; *Implementation of Codes*
  - [ ] &nbsp; *Running Experiments*
  - [ ] &nbsp; *Record Results*
  - [ ] &nbsp; *Analysis Results* 

## Document-wise Memories 



## Experimental Settings 

We use the same setting in [the previous work](https://fxnnxc.github.io/study/240328_fever_neural_memory/) except the number of documents to 449 pages. 
We do not train the template for fever training dataset and directly evaluate the validation question and answer pairs by memorizing the evidence Wiki pages. 
We use Llama2 7B and  Llama2-Chat 7B for the memorization of documents. All the predictions are conducted with 1-shot setting where the few-shot examples are obtained from the training QA pairs of the same document. 


### Baselines and SimSet 

We use the following baselines for the experiment and evaluate the performance of SimSet structural memories for documents. 

1. Pretrained Status
2. Unstructured Neural Memory 
3. Structured with Disjoint Length Proportional 

We use the following Simset settings 
1. 1-shingling 
2. 2-shingling 
3. 3-shingling 

|Model            |  Pretrain |  Full  |  DLP |         |  1-shin  |    2-shin   |   3-shin   |    
|-----------------|-----------|--------|------|---------|----------|-------------|------------|
llama2 7b         |  0.435    |
llama2_chat 7b    |  0.556    |





