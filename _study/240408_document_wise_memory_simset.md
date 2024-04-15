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
  - [x] &nbsp; *Documentation of Settings*
  - [x] &nbsp; *Implementation of Codes*
  - [x] &nbsp; *Running Experiments*
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



|Model            |  Pretrained Weights |  Finetune Full Weights | Memory (24th)  |  1-shin (24th)   |    2-shin (24th)   |
|-----------------|---------------------|------------------------|----------------|------------------|--------------------|
|llama2_chat 7b   |        0.644        |         0.655          |                |                  |                    |




학습 시간 및 레이어에 따른 에러는 다음과 같다. 
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211520&authkey=%21ANxTllKjra8N740&width=716&height=726" width="716" height="726" />



학습 시간 및 레이어에 따른 에러를 시각화하면 다음과 같다. 
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211521&authkey=%21AASIkNN2b464Wsw&width=704&height=700" width="704" height="700" />


각 레이어에 대해서 추가적인 모듈을 학습하는 경우 다음과 같은 정확도를 가진다. 
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211535&authkey=%21AIp4mtZQFFSnv5g&width=660" width="660" height="auto" />


레이어, 문서 암기 에러, Factual Knowledge 정확도는 다음과 같다.
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211522&authkey=%21AL3pl1mGWgMZoFk&height=660" width="auto" height="660" />


## No Clear Sign of Benefits of SimSet 



<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211534&authkey=%21AK-qxNrFwF--dUw&width=874&height=729" width="874" height="729" />


<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211536&authkey=%21AFRTiDkjsX7FQHo&width=874&height=571" width="874" height="571" />


* 문서 암기를 추가적으로 하는 것은 Fever데이터에 대한 정확도를 증가시켰다. 
* Finetune vs Memory를 비교하면, 어떤 레이어를 설정하느냐에 따라서 결과가 다르게 나왔다. 일반적으로는 Full Finetuning보다 성능이 저하되었다. 


 
##  Analysis 

저장은 잘 되는 것 같다. 그러나 저장된 정보를 잘 활용하는지는 여전히 미지수로 남아있다. 이 정보를 활용하는 것을 추가적으로 학습하기 위해서도 어뎁테이션 학습이 가능하다. 따라서 만일 Simset이 별다른 추가 학습 없이 메모리를 효율적으로 사용할 수 있다면, 해당 메모리를 더욱 사용하도록 추가학습하는 것이 도움이 될 것이다. 

단순하게 저장하는 것이 도움이 된다는 점은 naive 하다. 
추가적으로 적합한 학습, 모델링 방법을 연구해야 한다. 저장 자체만으로는 부족하다. 

