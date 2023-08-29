---
layout: share-distill
title: 'Kolmar Software Overview'
date: 2023-08-28
description: 기본적인 머신러닝/딥러닝 프레임워크 및 실험 과정
---

## 1. 소프트웨어 목적

콜마 소프트웨어의 목적은 **지속적이고 자동적인 실험**을 지원하며, 새로운 데이터, 모델 구조, 실험을 지속적으로 관리하는 **딥러닝 프레임워크**를 제공하고, 학습된 모델로부터 인사이트 도출을 돕는데 있다. 
기본적인 구조는 다음과 같이 학습과 평가가 구분된 구조로, 학습에 사용하는 데이터와 테스트에 사용되는 데이터를 구분한다. **(1) 학습**과정에서는 선택된 학습 데이터, 모델, 모델 트레이너를 바탕으로 학습이 진행되며, 학습 과정에서 모델 체크포인트가 저장된다. 저장된 체크포인트는 **(2) 테스트**과정에서에서 테스트 데이터를 바탕으로 평가코드로 정량적 및 정성적 평가 결과들을 생성해낸다. 이로부터 저장된 학습 및 테스트 정보들은 레포트를 작성하는 소재가 된다. 

<figure>
<img src="https://drive.google.com/uc?export=view&id=1AjinezaQqG-mRg4IhT4QZzHjFX7w_v8X">
</figure>

사용자가 데이터셋, 모델, 학습방법, 평가데이터셋, 평가코드를 선택하면, **자동으로 모델의 학습부터 평가까지 진행되며**, 정량적 및 정성적으로 평가결과는 저장된다. 이로부터 전체 실험에 대해서 정보를 저장하고, 인사이트가 존재하는 경우, 수동적으로 레포트를 작성한다. 유저는 자동화된 소프트웨어로부터 학습부터 평가까지 진행하지만, 이로부터 레포트를 작성하는 것은 수동적으로 진행한다. 이러한 구분은 보다 딥러닝 모델로부터 지속적인 인사이트를 얻는데 “효율적인 정보”를 따로 추출해내는데 있다.

**(수동)**[선택:학습데이터, 모델, 테스트데이터, 평가방식] $\rightarrow$ **(자동)**[학습, 평가] $\rightarrow$ **(수동)**[레포트 작성]



해당 도안에는 딥러닝에 필수적인 *“데이터 전처리"*와 *"학습 세팅 (training configuration)"*이 포함되어 있지 않지만, 마찬가지로 버전으로 관리된다. 


## 2. Versioning: 다양한 데이터 종류, 모델 종류, 학습 결과 보고서

이에 본 소프트웨어는 지속적으로 업데이트될 데이터, 모델, 학습 방법, 학습 결과, 평가, 레포트에 대해서 **버전으로 관리하는 것을 목적으로 한다.** 버전 관리는 **데이터 및 모델이 지속적으로 업데이트** 되는 과제의 특성상 적합하며, 비개발자가 사용하는데 있어서, 요소들을 버전으로 선택함으로써 사용의 효율화를 돕는다. 
버전관리에 해당 하는 부분들은 다음과 같이 학습 및 평가를 구성하는 여러 항목들이다. 해당 항목들은 지속적인 연구 개발을 위해서 버전으로 관리되며, 지속적인 
모델 및 데이터셋 간의 성능 비교를 가능하게 만든다. 

<figure>
<img src="https://drive.google.com/uc?export=view&id=14Vx1064dr3wzyntBGfje0wd0yRn3Dall">
</figure>

*  <code style='background-color:#FFFFDD'>데이터셋</code> : 학습과 평가에 사용되는 데이터셋의 버전을 나타낸다. 
*  <code style='background-color:#DDFFDD'>모델</code>: 모델과 모델 학습에 사용되는 Trainer의 버전을 나타낸다. 
*  <code style='background-color:#DDEEFF'>평가</code> : 평가 및 레포트의 버전을 나타낸다. 

## 3. 데이터셋 예시 

데이터셋은 버전으로 관리되며, `dataset_v1` 와 같은 이름으로 버전을 관리한다. 예시로, `example_excel` 을 살펴보자. 
해당 데이터셋은 다음과 같이 입력과 출력을 위한 폴더가 따로 존재하며, 각 폴더 내에는 샘플들을 액셀파일로 저장되어 있다.  

<d-code block language="bash" style='font-size:`15px'>
- input (폴더📂)
    - sample_0.xlsx (샘플0 - 성분, 엑셀, 📊) 
    - sample_1.xlsx (샘플1 - 성분, 엑셀, 📊)
    - sample_2.xlsx (샘플2 - 성분, 엑셀, 📊)
    ...
- output (폴더📂)
    - sample_0.xlsx (샘플0 - 물성, 엑셀, 📊)
    - sample_1.xlsx (샘플1 - 물성, 엑셀, 📊)
    - sample_2.xlsx (샘플2 - 물성, 엑셀, 📊)
    ...
</d-code>

각 샘플의 입력 (input) 과 출력 (output)은 다음과 같은 형태이다. 
해당 형태는 임의로 작성된 것으로 실제 데이터셋은 실무자와 회의를 거쳐서 선택된다. 
총 9개의 랜덤 샘플을 생성한 데이터는 [[drive link](https://drive.google.com/drive/folders/1jKcLQoga0ScRa5mvJqkbMmgpWscJU3-i?usp=sharing)]에서 확인할 수 있다. 

<center>
    <table>
    <tr><td>input/sample_0.xlsx</td><td>output/sample_0.xlsx</td></tr>
    <tr>
    <td> <iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vSB5oP_dVkjaOGK2B4Xhj1pHVgsB5J2RXIHZ_ftg5BnzXE_LBY0jUctdIS5fX3sSw/pubhtml?widget=true&amp;headers=false" height="300px" width="100%" >
    </iframe>
    </td> 
    <td> <iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vQvPoHj4AM6ZoWm2U49Exw1wMhnGwQFQhnTcH8_38BOqC69knJvNARGKWqqM0It0Q/pubhtml?widget=true&amp;headers=false" height="300px" width="100%" >
    </iframe>
    </td> 
    </tr>
    </table>
</center>

## 추가적인 발전 사항 (확장성)

딥러닝으로부터 인사이트를 얻기 위해서는 모델을 활용하여 다양한 Task<d-footnote> 학습과 테스트는 모델을 다루는 한 가지 방법이다. </d-footnote>를 생성해야 한다. 본 소프트웨어 소개에서는 학습-테스트에 초점을 맞춰서 소프트웨어를 설명하였지만, 실제 소프트웨어는 더욱 다양한 기능들을 포함할 수 있다. 예시는 다음과 같다. 


* Domain Adaptation 
* Transfer Learning 
* Representation Learning 
* hierarchical feature learning 
* XAI (설명가능인공지능)
* Interpretability (딥러닝 모델 해석)


이러한 테스크들을 지원하기 위해서 본 소프트웨어는 학습과 테스트에 대해서 (학습-테스크) 및 (테스트-테스크)로 명칭하며, 이후 추가적인 테스크들이 필요성을 의논하고 개발될 예정이다. 

---


