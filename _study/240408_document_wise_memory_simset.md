---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-04-14
featured: true
title: '문서 메모리 방식은 document memorization으로 Factuality에 대해서 높은 성능을 낼 수 있는가? (Korean)'
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

문서에 대한 메모리를 설정하는 것은 지식을 더욱 정렬하는 효과를 불러일으킨다.
이 연구에서는 동일한 파라미터에 대해서 문서별 메모리를 설정하는 것이 비슷한 문서 암기 수준을 보인다는 점을 밝힌다. 
그러나, Factuality Question Answering에 대해서는 문서에 저장된 정보를 제대로 활용하지 못한다고 생각한다. 

몇 개의 레이어에서는 축자ㅓㄱ인 학습 없이도 문서의 메모리를 


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


⚠️ 햐당 결과는 memory 와 simset의 메모리 구조가 다르다. memory 는 layer_norm 과 gating을 추가로 사용하였다. 

|Model            |  Pretrained Weights | Memory (24th)  |  1-shin (24th-dim) | 1-shin (24th-token)   |    2-shin (24th-dim) | 2-shin (24th-token) |
|-----------------|---------------------|----------------|--------------------|-----------------------| -------------------- | --------------------|
|llama2_chat 7b   |        0.644        |  0.652         |   0.640            |        0.641          | 0.640                | 0.653               |


아래 결과는 simset에 대해서 memory와 동일한 모델 구조를 사용한 결과이다. 

|Model            |  Pretrained Weights | Memory (24th)  |  1-shin (24th-dim) | 1-shin (24th-token)   |    2-shin (24th-dim) | 2-shin (24th-token) |
|-----------------|---------------------|----------------|--------------------|-----------------------| -------------------- | --------------------|
|llama2_chat 7b   |        0.644        |  0.652         |


#### 문서 암기 학습 에러

학습 시간 및 레이어에 따른 에러는 다음과 같다. 마지막 레이어에 가까워질수록 loss와 가깝기 때문에 더 낮은 에러에서 시작한다. 그러나 학습이 오래 진행될수록 일반화 성능은 떨어지는 것을 확인할 수 있다. 
중간 레이어를 수정하는 것은 모델의 state에 대해서 수정하기 때문에 장기적으로 봤을 때 에러가 더 낮은 것으로 보인다. 따라서 중간 레이어에 대해서 추가 메모리 구조를 적용하는 것은 **GPT 내부 state에 대한 변화를 주는 것이다.** 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211520&authkey=%21ANxTllKjra8N740&width=716&height=726" width="500" height="700" style="width:49%" />
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211521&authkey=%21AASIkNN2b464Wsw&width=704&height=700" width="500" height="700"  style="width:49%"/>

#### FEVER 정확도 

각 레이어에 대해서 추가적인 모듈을 학습하는 경우 다음과 같은 정확도를 가진다. Pretrained는 모델에 다른 작업 없이 1-shot으로 예측하는 결과이다. 
대부분의 레어어들은 모델에 adaptation하는 것이 오히려 성능 저하를 가져왔다. 반면 특정 
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211535&authkey=%21AIp4mtZQFFSnv5g&width=660" width="660" height="auto"  style="border:#000000 solid 2px" />



레이어, 문서 암기 에러, Factual Knowledge 정확도는 다음과 같다.
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211522&authkey=%21AL3pl1mGWgMZoFk&height=660" width="auto" height="660" />


## No Clear Sign of Benefits of SimSet 

그냥 메모리에 저장하는 것은 모델이 알아서 사용하도록 큰 도움을 주는가? 내가 표현을 적절하게 바꿔주는가? 그렇지 않은 것 같다. 
일부 모델링에서 약간의 성능 향상이 있지만, 유의미한 수준이 아니며, 일반적으로는 성능 저하가 나타나는 것으로 보인다. 
따라서, 지식을 저장하는 것과 LLM이 저장된 지식 표현을 효율적으로 사용하는 것을 보장하지 않는다. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211534&authkey=%21AK-qxNrFwF--dUw&width=874&height=729" width="874" height="729" />


SimSet-Dimension의 경우 Loss 자체는 높은 수준을 보였기에 저장이 제대로 되지 않았다고 할 수 있다. 그러나, Accuracy 자체는 더 높게 나타났기에 저장이 잘 된다고, 혹은 유의미하게 된다고 
다른 Task에 모델이 바로 사용가능하다는 것은 틀렸다. 모델이 저장된 지식을 예측에 사용하게 만들기 위해서 적절한 모델링이 추가적으로 연구되어야 한다. 


<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211536&authkey=%21AFRTiDkjsX7FQHo&width=874&height=571" width="874" height="571" />


* 문서 암기를 추가적으로 하는 것은 몇몇 레이어에서 Fever 데이터에 대한 정확도를 증가시켰다. 그러나 일반적으로 모든 레이어에서 오르지 않으며, 구조적으로 설계된 메모리가 정확도를 올리지 않는다. 
 
##  Analysis 

저장은 잘 되는 것 같다. 그러나 저장된 정보를 잘 활용하는지는 여전히 미지수로 남아있다. 
이 정보를 활용하는 것을 추가적으로 학습하기 위해서도 어뎁테이션 학습이 가능하다. 따라서 만일 Simset이 별다른 추가 학습 없이 메모리를 효율적으로 사용할 수 있다면, 해당 메모리를 더욱 사용하도록 추가학습하는 것이 도움이 될 것이다. 

단순하게 저장하는 것이 도움이 된다는 점은 naive 하다. 추가적으로 저장된 정보를 활용하는 방법을 연구해야 한다.  저장 자체만으로는 부족하다. 
정보 저장의 관점에서는 전체 메모리를 사용하는 것과 동일한 수준으로 암기력을 보이고 있다. 그러나 해당 정보가 실제로 더욱 도움이 되는지는 아직 부족하다. 
따라서, Neural 메모리의 효율을 올리기 위한 구조적인, 알고리즘적인 방법을 찾아야 한다. 