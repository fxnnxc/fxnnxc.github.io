---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2024-02-21
featured: true
title: '[한국어] X-Ray Object Detection and Saliency'
description: 'Running saliency explanation with Yolo'
img: 'https://onedrive.live.com/embed?resid=AE042A624064F8CA%211018&authkey=%21APfArBZvQkN9CO4&width=445&height=401'
---

## X-Ray Object Detection and XAI

* 프로젝트 Report Code [v24.02.22.1](https://github.com/fxnnxc/multiobject_xai/tree/v24.02.22.1)


### Object Detection 연구 개요

OD 연구는 물체 예측을 하는 2012년 CNN이후 Object Detection Task에서 물체 탐지를 위한 연구로 진행되었다. 
신경망을 활용한 방식은 크게 candidate sampling 의 여부로 one-stage 와 two-stage 로 나뉜다. 두 가지 방식 중 온라인에서 더욱 빠르게 예측하며 신경망 의사결정을 설명하기에 적합한 방식은 one-stage 이다. 대표적인 one-stage인 YOLO는 2014년 YOLOv1 을 시작으로 2023년 YOLOv8까지 모델이 나왔다. 

YOLO Architecture는 전체적인 프레임워크 변화는 적으나 각 모델별로 output 의 형태가 일치하지 않는다. 이러한 output-형태의 비정형화는 설명 알고리즘을 설계하는데 고려되어야 한다. 왜냐하면, 의사결정을 설명하는 인공지능이 무엇을 의사결정으로 칭하는지에 따라서 설명이 달라지기 때문이다. YOLOv3와 YOLOv8의 경우 모델의 아웃풋은 다음과 같은 형태로 되어있다. 
 
|모델 | 아웃풋 형태 | Anchor Box 
|---|---|---|
| YOLOv5 |  [batch_size, number_of_anchors, 4 + 1 + number_of_classes] | Anchor Box 
| YOLOv8 |  [batch_size, number_of_anchors, 4 + number_of_classes] | Anchor Box Free


출력값의 형태가 다를지라도, XAI기술 중 일부는 모델의 의사결정을 설명하는데 적용될 수 있다.  인공지능 설명 기술 XAI는 모델의 타입에 크게 변화하지 않는 model-agnostic 한 saliency map 혹은 CAM기반이 적합하다. 이는 앞으로도 OD 관련 모델이 개선되도 사용가능한 XAI기술의 적용이 장려되기 떄문이다. 최근에는 ViT모델을 활용한 OD 연구도 진행되었는데, 이 모델에 대해서 Saliency Map과 CAM은 적합하지 않으며, Transformer에 특화된 XAI기술 (attention, Transformer LRP)와 같은 기술에 대한 설명 알고리즘 연구가 필요하다. 


###  X-ray + OD 특징 

X-ray에서 설명이 필요한 이유는 대부분의 객체가 overlapping되어 있기 때문이다. object detection 모델은 대부분 물체에 대한 bounding-box와 레이블을 제공해주며, 객체가 겹쳐있을 경우 물체의 인식은 다중 객체이다. 이에 대한 대책으로 XAI기술은 의사결정에 대해서 attribution map을 제공하여 의사결정을 보조하는 역할을 할 수 있다. 이러한 한계를 개선하기 위해서 다중 시점에 대한 물체탐지가 가능하며, 이 경우도 설명을 제공하면 감시자 입장에서 물체 탐지에 대한 경험을 바탕으로 의사결정을 보좌할 수 있다. 


## 데이터 

학습데이터는 Robotflow에서 제공하는 세 종류의 위험물 X-Ray데이터 SIXRay, Threat, Prohibited를 사용하
였다. 아래 표는 세 종류의 데이터에 대한 학습, 평가, 테스트 데이터의 수는 다음과 같다. 

|  Data | train | valid | test 
|---|---|---|
| [SIXRay](https://universe.roboflow.com/siewchinyip-outlook-my/sixray/dataset/4) | 5819 | 1662 | 831
| [Threat](https://universe.roboflow.com/gmrit/threat-detection-9yn3r) | 2518 | 298 | 152
| [Prohibited](https://universe.roboflow.com/deeplearning-xray-model-training/prohibited-items-x-rayyolov8/dataset/2346) | 8685  | 1660 | 797

## 학습 결과

YOLO 모델의 학습은 50 Epoch을 진행하였다. YOLO 모델의 세가지 데이터에 대한 성능은 아래 표와 같다.
성능지표는 mAP50 (mean Average Precision), mAP50-95(0.5부터 0.95까지)측정을 사용하였다.

|  Data | YOLOv5 mAP50 | YOLOv8 mAP50  | YOLOv5 mAP50-95 | YOLOv8 mAP50-95
|---|:-:|:-:|:-:|:-:|
| SIXRay | 88.07 | 88.84 | 61.22 | 62.37 |
| Threat | 96.98 |  96.91 | 85.91 | 85.27 |
| Prohibited | 88.57 | 90.07  |  63.38 | 65.27 | 

## XAI 결과 

아래 그림은 SIXRay 데이터와 Threat 데이터에 대한 saliency map 예시를 보여준다. 

<div style="display: grid;grid-template-columns: 1fr 1fr;border:1px solid #DDDDFF;border-radius:10px;padding:10px;align-items:center;">
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211028&authkey=%21AKkOS-BUvRebBAM&width=482&height=464" width="100%" height="464" />
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211029&authkey=%21AEZUNaB2czJM-z0&width=474&height=456" width="100%" height="456" />
</div>


## XAI 효과 

<div style="display: grid;grid-template-columns: 1fr 1fr;border:1px solid #DDDDFF;border-radius:10px;padding:10px;align-items:center;"  markdown="1">
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211032&authkey=%21AKk1oLGjmaWJM3s&width=368&height=155" width="368" height="155" />
<div markdown="1">
**(XAI가 객체 탐지의 중요부위를 보여준다.)** <br>
* 예측 결과: 렌치
* XAI 결과: 렌치의 끝, 중간 부분, 칼의 손잡이 

위험물의 어떤 부분이 예측에 작용하였는지 보여준다. XAI 기술이 렌치의 머리 부분을 강조하기 때문에, 관찰자는 해당 부분을 고려할 수 있다. 
</div>

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211030&authkey=%21AI9uaXieW1U42Hw&width=375&height=155" width="375" height="155" />
<div markdown="1">
**(XAI가 객체 탐지의 중요부위를 보여준다.)** <br>
<ul>
<li> 예측 결과: 없음 </li>
<li> XAI 결과: 아래 하단 물체 </li>
</ul>
객체 탐지는 이루어지지 않았다. 물체의 확신이 낮았기 때문이다. XAI결과 아래쪽 물체의 기여가 존재했기에 관찰자는 해당 물체를 재고려할 수 있다. 
</div>

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211031&authkey=%21AGkVgbStpYmlxxM&width=327&height=171" width="327" height="171" />
<div markdown="1">
**(XAI가 객체 탐지의 중요부위를 보여준다.)** <br>
* 예측 결과: 칼
* XAI 결과: 칼날과 손잡이 중간 부분

칼로 예측한 근거는 칼날과 손잡이 중간 부분이 다르기 떄문이다. XAI 정보를 바탕으로 관찰자는 해당 부분을 집중적으로 고려할 수 있다. 
(손잡이 부분의 기여도는 수리검의 끝부분으로 발생한 것이다.)  
</div>

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211033&authkey=%21AHHlkoNIaio-ryc&width=231&height=145" width="231" height="145" />
<div markdown="1">
**(수리검의 bounding box 기여)** <br>
* 예측 결과: 표창 (95% 신뢰도)
* XAI 결과: 표창의 끝 부분

모델의 물체의 bounding box를 추정하도록 학습되었다. 표창의 끝부분은 물체를 탐지하는 것과 물체의 bounding box를 결정하는 역할을 하였다. 
</div>

</div>

###  추후 연구 방향

연구결과 객체 탐지에 대해서 인공지능 설명 기술이 모델의 의사결정에 대한 정보를 제공하여, 
관찰자가 입력 이미지, 의사결정, 의사결정 해석으로 더 좋은 의사결정을 보일 가능성을 보였다. 
현재 한계는 설명기술의 정확도가 불완전하며, 고도화되지 않는 경우, 잘못된 의사결정 설명을 줄 수 있다. 

현재 수준보다 해석 수준을 높이기 위해서는 1) 고도화된 XAI기술을 사용하여 위험물체 탐지에 적합한 설명을 제공하고, 2) 다중물체탐지에 적합한 XAI 기술을 개발해야 한다. 설명 기술의 정성적 평가 및 object detection에 대해서 적합한 설명 기술이 필요하다. 




