---
layout: share-distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2024-01-28
featured: true
img: 
title: 'Experimental Design'
description: ''
_styles: >
    .table {
        padding-top:200px;
        margin-bottom: 2.5rem;
        border-bottom: 2px;
    }
    .p {
        font-size:20px;
    }

---

## 연구 과정 

1. [문서-태깅 학습] 모델에서 문서별 학습에 대해서 문서를 학습하면서 원천 태깅을 하도록 표현학습을 할 수 있는가? 
2. [문서 혼합 일반화] 문서에 대해서 섞인 컨텐츠의 문서 추정 기능이 유지되는가?
3. [컨텐츠 일반화] 문서에 없는 내용에 대한 반응 


## 구성 단계

1. 문서 태깅과 함께 학습한다. 
2. 문서 태깅 없이 생성한다. 
3. 원래 문서의 추정 수준과 PPL 을 측정한다. 


## 실험에 대한 고민 



## 필요한 학습 Objective 들 


1. Language Modeling 
2. 문서 추정
3. 문서 태깅 없이 표현 일치
