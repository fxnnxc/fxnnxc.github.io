---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2024-09-17
featured: true
title: 'Unexpected Good Output Tree'
description: ''
tags: neuron
---

---

## SteeringTree 

- Writing introduction (24.09.17)
  - Contribution 및 분석 흐름 완성 (24.09.17) ✅
  - Steering location에 따른 in-context 오작동 문제 발견 - logit이면 in-context가 안됨. (24.09.17)
    
- Writing Methods (24.09.17) 
  - 🧷[정의 1] Hidden representation 기반 Notation (24.09.17) 
    - Causal Indirect Effect by Binarized Prediction 만듬. (24.09.17) ✅
    - Notation 정리 깔끔하게 해야 됨. (24.09.17) 😡 
      - 데이터 $\mathcal{D} = \{ (x_i, y_i) \}_i$ (24.09.17)
      - In-context Prompt 등 (24.09.17)
      - Measure 값 정의 등  (24.09.17)
  - 🧷[정의 2] Steering location에 따른 표현값
    - 단일 위치 / 다중 위치 노테이션 
  - 🧷[정의 3] Contribution of Steering 정의 
  - 🧷[정의 4] Mitigating Steered Bias 
    - Drop K, V 
    - (다른 대안을 하나 더 찾자.)
- Writing Experiments (24.09.17) 
  - dataset preparation 
  - steering vector computation
  - in-context 기반 세팅 (**몇 가지 경우들이 있는지 확인 필요!**) 😡 



---

## Kolmar Preparation 

- Papers: Viscosity Prediction 
  - A: A Machine Learning Approach to Predict Fluid Viscosity Based on Droplet Dynamics Features 
    - [link](https://www.mdpi.com/2076-3417/14/9/3537)
    -  Three machine learning models based on multivariate linear regression, random forest, and neural network
    -  Droplet dynamics features는 액체 방울이 움직일 때나 다른 요소와 상호작용할 때 발생하는 물리적 특성
  - B: Application of machine learning techniques to predict viscosity of polymer solutions for enhanced oil recovery
    - [link](https://link.springer.com/article/10.1007/s12667-023-00635-7)
    - 다중 선형 회귀(MLR), 서포트 벡터 머신(SVM), 회귀 의사결정 트리(RDT), 인공신경망(ANN)
    - 문제의 비선형성으로 인해 MLR은 폴리머 점도 예측에 적합하지 않다고 결론지었습니다. 기계 학습 방법 중에서는 두 개의 은닉층과 각 층에 다섯 개의 뉴런을 가진 인공신경망(ANN) 모델이 훈련, 검증, 테스트 데이터 세트 모두에서 상관 계수 0.99를 달성
  - C: Facial Beauty Prediction Based on Vision Transformer
    - https://www.ijeetc.com/index.php?m=content&c=index&a=show&catid=189&id=1791
    - 이미지 기반 
  - D: Machine Learning-Based Facial Beauty Prediction and Analysis of Frontal Facial Images Using Facial Landmarks and Traditional Image Descriptors
  - E: Cosmetics Suggestion System using Deep Learning 
  - F: A fingerprints based molecular property prediction method using the BERT model
    -  약물 발견과 재배치에 중요한 분자 속성 예측(MPP) 
    - 제안된 모델은 분자 서열 임베딩과 예측 모델로, 분자 속성 관련 특징을 추출하기 위해 BERT 기반의 Fingerprints-BERT(FP-BERT)를 사전 학습하여 분자의 의미적 표현을 획득. 
    - FP-BERT로 인코딩된 분자 표현을 CNN에 입력해 더 추상적인 고차원 특징을 추출한 후, 분자 속성을 분류 또는 회귀 작업으로 예측
  - G: Mol-BERT: An Effective Molecular Representation with BERT for Molecular Property Prediction 
    - 제안된 Mol-BERT 모델은 약물 분자의 SMILES 데이터를 기반으로 분자 구조 정보를 추출하는 BERT 모델을 사전 학습 
    - 약 400만 개의 비라벨링된 약물 데이터를 사용해 사전 학습된 이 모델은 다양한 분자 속성 예측 작업에 맞게 미세 조정될 수 있습니다.
  - MPP 문제 
    - SMILES (Simplified Molecular Input Line Entry System): •	분자의 구조를 1차원 문자열로 표현하는 방식입니다. SMILES는 분자 구조의 원자, 결합, 방향성 등을 문자열 형태로 나타내며, 머신러닝 모델에서 자주 사용됩니다. •	예: 물(H₂O)의 SMILES 표현은 “O”입니다.
    - Fingerprint (분자 지문): •	분자의 구조적 특징을 고정된 크기의 이진 벡터로 변환한 것입니다. 각 비트는 분자의 특정 구조적 서브패턴의 존재 여부를 나타냅니다. •	Morgan Fingerprint와 같은 방식이 많이 사용됩니다.

---


<style>
    h2::before {
      content: "🌲";
    }

    /* 1레벨: 기본 원형 */
    ul {
      list-style-type: ;
    }
    ul > li{
      font-weight: bold;
    }
    ul ul > li{
      font-weight: normal;
    }

    ul > li::before {
          content: "🌲 "; /* 체크 마크 이모지 */
        }
    /* 2레벨: 별 이모지 */
    ul ul > li::before {
      content: "→ "; /* 별 이모지 */
    }

    /* 3레벨: 하트 이모지 */
    ul ul ul > li::before {
      content: "→ "; /* 하트 이모지 */
    }
    ul ul ul ul > li::before {
      content: "→ "; /* 하트 이모지 */
    }
    ul {
      list-style-type: none;
    }

    /* 2레벨: 빈 원형 */
    ul ul {
      list-style-type: none;
    }

    /* 3레벨: 사각형 */
    ul ul ul {
      list-style-type: none;
    }
</style>

