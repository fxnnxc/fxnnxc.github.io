---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-04-16
featured: true
title: 'Simset + inductive bias regularization (Korean)'
description: 'Simset에 대해서 제대로 연구하기 위해서 inductive bias를 위한 실험 설계 및 진행.'
---
## Does Structural Document Memory Help Factual Recall? 

* 🗓️ **Experiment Date** : 24.04.17
* 🌸 **Scope** : FEVER dataset and counterfactual dataset construction and experiments. 
* 🧑🏻‍💻 **Code**:[Simset]()
* 😮 **Status** : Running Experiments 
  - [ ] &nbsp; *Documentation of Settings*
  - [ ] &nbsp; *Implementation of Codes*
  - [ ] &nbsp; *Running Experiments*
  - [ ] &nbsp; *Record Results*
  - [ ] &nbsp; *Analysis Results* 


## Questions 

1. Inductive Structure 메모리를 사용하는 것은 암기 속도를 올리는가? 
2. Inductive Structure 메모리로 학습된 정보는 문서 관련된 QA에 대한 정확도를 높이는가? 


## Data Processing Pipeline 

We use two datasets, CounterFact and FEVER datasets.
For the related documents of CounterFact, we find wiki pages related to the subject word in Counterfact including single topic which has 1285 documents.  For FEVER data, the evidence page for paper_dev dataset which includes 449 documents. For additional data processing, we provide details in the Appendix. 

### FEVER Documents


### CounterFact Documents


## Baselines and Proposed Methods

For all cases, we use the memory size of 1024, 4096, 12288. 
For all cases, we use four types of model architectures: Llama2 7B, Llama2 13B, Pythia 6.9B, Pythia 1B

* Baseline 1: no inductive entry bias
* Baseline 2: random entry bias
* Baseline 3: fixed partitioned entry bias
* Baseline 4: MemTRN (for future work)
* Proposed 1: Simset-1
* Proposed 2: Simset-2




----

## Experiment 1


### Training 1 (FEVER)

* Dataset: FEVER 

Document training for fixed epochs and optimizers

#### Evaluation 1 (FEVER) 

* Measure: negative log likelihood in training
* Models: Pythia, Llama

#### Evaluation 2 (FEVER)

* Measure: F1 score (a,b,c) The first character.  ` (a) ` 
* Models: Pythia, Llama

Document training for fixed epochs and optimizers

----

### Training 2 (Counterfact)

* Dataset: CounterFact

Document training for fixed epochs and optimizers

#### Evaluation 1 (Counterfact)

* Measure: negative log likelihood in training
* Models: Pythia, Llama



#### Evaluation 2 (Counterfact)

* Measure: F1 score for the target true (sampling five answer)
* Models: Pythia, Llama

Document training for fixed epochs and optimizers

----

## Experiment 2: Clustering Documents

위 실험 중에 제일 적합한 데이터-모델에 대해서 문서 클러스터링을 했을 때 효과. 
중점: 클러스터를 형성하면 성능이 저하되는가? 향상되는가? 


## Experiment 3: Regularization Alpha에 대한 정도 

* 0에 가까울수록 no inductive bias 
* 클수록 inductive bias가 더 강해진다. 



## Experiment 4: Running on different Layer

For all cases, we evaluate validate the 10 internal layers. 
