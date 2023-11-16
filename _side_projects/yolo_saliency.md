---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-11-15
featured: true
title: '[Korean] Saliency Map with Object Detection Models'
description: 'Running saliency explanation with Yolo'
img: 'https://drive.google.com/uc?export=view&id=1Ex_iwYd-961_w1IFUL1f5O72mm2qZy0r'
---


## Object Detection and XAI


* 프로젝트 Report Code [v23.11.16.1](https://github.com/fxnnxc/multiobject_xai/tree/v23.11.16.1)


그림 1은 예측 모델과 예측을 해석해주는 설명 가능 모듈 프레임워크를 보여준다. 위험물품에 대한 종류를 예측 모델이 판단하면, 사용자는 일차적으로 물품에 대한 레이블을 제공 받는다. 이후, 설명 가능 모듈은 (1) 이미지, (2) 신경망 모델, (3) 예측 결과를 바탕으로 ‘모델의 의사결정’을 설명할 정보를 사용자에게 제공한다. 사용자는 (1) 입력데이터에 대한 설명 결과를 받는다. 이로부터 이미지의 어떤 부분이 위험물로 분류되도록 기여한 위치를 판단할 수 있다. 예를 들어, 칼을 탐지한다면, 날카로운 칼날 부분에 대한 기여가 높게 나온다. 설명성은 또한 (2) 모델에 대하여 동작을 검증하는데 사용될 수 있으며, (3) 예측 결과 자체에 대한 신뢰도를 제공해준다. 


<img src="https://drive.google.com/uc?export=view&id=1QjPzAFtwuBB6wSTmfGdJeAFh3bGTEkve" style="width:100%;margin-left:0rem;border:1px;"  >


## Saliency


2. 가장 많이 사용되는 객체탐지모델 YoloV3의 예측과 예측에 대한 설명성 결과는 그림 2와 같다. 설명성 방식은 saliency-map 종류인 integrated gradient를 사용하였다. 예측 모델의 결과는 (가)처럼 전반적인 영역을 보여주며, (나) 설명 결과는 객체를 판단할 때 주요했던 영역을 보여준다. 사람의 경우 얼굴 부분이 가장 큰 기여를 한 것을 확인할 수 있다. 이와 유사하게 위험물에 대한 설명은 위험물의 어떤 부분이 판단의 근거로 작용하였는지 확인시켜주며, 예측 결과의 신뢰성을 높인다. 


<img src="https://drive.google.com/uc?export=view&id=1bsx9I7HlTKW6rDH6rxeeflR6wG7MThDH" style="width:100%;margin-left:0rem;border:1px;"  >


## Utility

3. 설명성을 활용하는 시나리오는 여러 가지가 있다. 그림 3의 첫 번째 이미지는 얼룩말을 기린으로 오인한 결과를 보여준다. 기린에 대해서 얼굴과 목 부분에 설명이 존재하는 것과 대비되게 얼룩말에는 기린에 대한 근거가 존재하지 않는다. 따라서, 모델의 예측 결과인 “기린”을 신뢰할 근거가 존재하지 않음을 설명 가능 인공지능을 통해서 보였다. 결과적으로 예측과 설명 두 가지를 기반으로 예측의 신뢰성을 더욱 높일 수 있다.

<img src="https://drive.google.com/uc?export=view&id=1EUm8rbnE3yOyhTn2yW4NBjHrtFVWs-tf" style="width:100%;margin-left:0rem;border:1px;"  >

<img src="https://drive.google.com/uc?export=view&id=1j8CHVBVZF2ogEuMPfIwGYPumdovbmiAl" style="width:100%;margin-left:0rem;border:1px;"  >


## + ParchGrad

<img src="https://drive.google.com/uc?export=view&id=1srJg5F-kWoeLJzHtMf_a3xOvcjb3WP5a" style="width:100%;margin-left:0rem;border:1px;"  >

