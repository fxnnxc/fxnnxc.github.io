---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2024-03-03
featured: true
img: 
toc:
  - name: Introduction
title: 'Memory as the Computational Resource'
description: 'Study note'
---


## Introduction 

* [youtube](https://www.youtube.com/watch?v=--vC_MXcbFA&list=LL&index=3)


메모리는 적고, 모델은 많고 데이터는 더 많다. 
학습을 위해서는 메모리가 필요하다. 알고리즘은 여러번 learning process를 진행하며, 정보를 저장하기 위해서 적절한 메모리를 가지고 있어야 한다. 
데이터와 모델이 기하급수적으로 늘어난 것과 대비하여 메모리 수는 굉장히 천천히 증가하였다. 

학습과 최적화 입장에서 메모리의 역할은 무엇인가? 
메모리와 required information 의 양은 특정한 관계가 있는가? (# gradient queries, # data points)

Memory Dichotomy Hypothesis: It is not possible to significantly improve on the convergence rate of known memory efficient techniques without using significantly more memory. 

----

A canonical optimization problem finding $min F(x)$.  
* Given access to a first-order oracle : algorithm queries some point $x$.  (gradient query)
* Oracle responds with $(F(x), \nabla F(x))$.  

Known for $d$ dimensional vector. 
* $\mathcal{O}(d)$ computation time per query for gradient descent 
* $\mathcal{O}(d)$ memory per query 
* query complexity : for the desired error $\epsilon$, need $\epsilon^{-2}$ queries to find $\epsilon$ optimal answer. 

위의 경우 query 자체는 에러에 대해서 제곱으로 늘어나게 된다. 따라서 에러를 충분히 낮추기 위해서는 무수히 많은 쿼리가 필요하다. 

Other techniques such as binary search 
* $> d^2$ computation time per query 
* $> d^2$ memory per query 
* query complexity : $d \log (\frac{1}{\epsilon})$  (cutting plane / ellipsoid methods)

다른 방법을 사용하는 경우 query에 대해서 많은 연산과 메모리가 필요하지만, 에러에 대해서 더 적은 쿼리가 필요하다. 따라서 쿼리 수를 줄이기 위한 적절한 방법이다.

알고리즘 자체는 효율적으로 구성될 수 있으며, 특정한 목적을 지니고 있다. 
메모리를 더 많이 쓰면서 더 효율적인 알고리즘을 구성할 수 있다. 

* lower bound of query:  정보이론적으로 항상 $d \log \frac{1}{\epsilon}$ 쿼리가 필요하다. 
* lower bound of memory:  정보이론적으로 $d$ 메모리 이상이 필요하다. 

----

Orthogonal Vector Game 

Random Matrix $A \sim Unif(\{\pm 1}^{\frac{d}{2} \times d})$
To win, find $y_1, y_2, \cdots, y_k$ which are roughly orthogonal to $A$. 

$M$-bit 메시지를 보내서 oracle 은 row number of largest inner product 를 준다. 
두 번째 쿼리를 보내면서 계속 가장 작은 row 를 반환해준다. 
이 작업을 $m$번 진행해서 $y_1, y_2, \cdots, y_k$ 개를 구해야 한다. 

Communication Game 

Transformer 는 쿼드라틱 메모리를 사용한다. 따라서 더 적은 샘플이 필요하다. 
LSTM은 linear 메모리를 사용한다. 더 많은 샘플이 필요하다. (쿼리 수가 많아야 한다.)



Transmission of center and move and scatter them again. 
Transmitting information directly and safely. 