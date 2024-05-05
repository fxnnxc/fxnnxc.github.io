---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-05-05
featured: true
title: 'Neuron Resampling Experiments on Sparse Autoencoding (SAE)'
description: '신경망 내부의 특징을 찾아내는 SAE에 대해서 neuron resampling기법을 적용하였다.'
---

## 소개글 




## 실험

### Classification + Random Resampling 

뉴런 `resampling`은 학습 과정에서 버려지는 뉴런에 대한 초기화를 진행하기 위해서 연구하였다. 

1. 너무 많이 켜진다면, 중요한 것과 중요하지 않는 것을 `구분`하지 않는다. 
2. 너무 적게 켜진다면, 데이터에 대해서 `충분히 학습되지 않은` 뉴런이라고 볼 수 있다. 값을 다음 레이어로 전달한 횟수가 적기 때문에 OOD와 같은 영향이 있다.  

따라서, 모델의 내부에서 Resampling이 필요한 뉴런을 찾아내고, 적절한 표현으로 바꾸는 것은 중요하다. 1) 뉴런을 찾아내는 것, 2) 뉴런을 수정하는 것, 3)  이후 영향을 분석하는 것 등 고려해야 하는 것들이 많다. 완벽에 가까운 표현 기계를 만들기 위해서는 모델의 내부를 원하는 대로 수정하거나 고칠 수 있어야 하기에, 이번에는 뉴런 resampling을 연구하였다. 

먼저, classification에 대해서 데이터에 대해서 켜지는 횟수가 20% 미만은 뉴런들을 지속적으로 초기화를 해주는 실험을 진행하였다. 해당 뉴런들은 학습과정에서 버려지는 뉴런들로 고려하였고, 버려지는 뉴런이 존재하면 모델의 표현력이 저하되므로 이를 개선하기 위해서 resampling을 테스트하였다. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%218853&authkey=%21AK_YNBEVcfahZq0&width=660" width="660" height="auto" />

실험결과, 뉴런의 개수가 충분히 많다면, resampling이 굳이 필요하지 않았다. 
그러나, 뉴런 개수가 적다면 resampling을 통해서 더 빠른 학습이 가능했다.  
해당 실험에서는 gate 부분에 50개의 뉴런을 가정했다. 

또한 너무 자주 리셈플링하면 좋지 않은데, 학습을 방해할 수 있기 때문이다. 모델은 momentum을 활용해서 loss가 낮아지는 방향으로 움직인데, 그런데 뉴런 리샘플링을 통해서 오나전히 새롭게 초기화되는 경우, 모델은 새로운 표현에 적응해야 한다. 이 경우, 학습 속도가 더욱 빨라지지 않는다. 뉴런 리셈플링의 목적은 죽은 뉴런을 살리는 것이고 이후 학습을 위해서 충분한 시간을 줘야 한다. 


* code for neuron resampling and training [[gist](https://gist.github.com/fxnnxc/51df0e0f44474c51b8186a673457b9d1)]

<d-code language="python">
# base neuron resampling (normal distribution)
def resample_linear_neuron_weight(linear, row):
    with torch.no_grad():
        std = linear.weight.data.std(dim=1).mean()
    linear.weight[row].data.fill_(1.0)
    linear.weight[row].data *= torch.randn_like(linear.weight[row])*std
    linear.bias.data[row] = 0.0
</d-code>

### Sparse AutoEncoder + Error Based Sampling 

알고리즘은 다음과 같이 동작한다. [[Towards Monosemanticity](https://transformer-circuits.pub/2023/monosemantic-features) 참조]

1. 활성화되지 않은 뉴런을 고른다. (list)
2. 에러가 높은 샘플들의 표현을 고른다. 에러에 비례하여 확률 분포를 이룬다. 
3. 활성화되지 않은 뉴런들을 차례대로 접근하여, 인코더와 디코더를 초기화한다. 
4. 인코더 초기화 : 해당 입력이 들어왔을 때, 활성화되도록 동일한 값으로 초기화 한다. 이 경우, neuron의 fire 값은 1이다. 
5. 디코더 : fire된 뉴런이 해당 값을 0.2만큼 선택하도록 L2 norm * 0.2로 dictionary에 넣어둔다. 나머지 0.8%는 다른 feature들이 활성화되서 결합된다. 

해당 알고리즘은 두 가지 가정을 가진다. 
1. 활성화되지 않는 뉴런이 존재한다. 
2. 활성화되지 않는 뉴런이 특정 데이터에 대해서 활성화 및 원래 복원을 20%만큼 가져간다. 

이 방식은 feature를 찾는다기보다 해당 입력이 그 자체로 feature라는 가정이 있는 것 같다. 