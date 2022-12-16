---
layout: post
title: 'Layer-wise Relevance Propagation'
date: 2022-04-15 11:12:00-0400
description: an example of a blog post with some math
tags: RL LRP XAI
categories: XAI-posts
---


한 가지 중요한 관점은 모델에 대해서 input attribution 을 고려 하는 것이다. 모델을 하나의 function $$f : (x_1, x_2, \cdots, x_p) \rightarrow \mathbb{R} $$ 으로 여겼을 때, 
각 input 들이 함수에 기여한 정도를 고려할 수 있다. 

해당 기여도를 모두 더할 경우, 원래의 함수값을 보존한다. 

기여도를 heatmap $$R(x)$$ 이라고 부르며, 각 pixel 위치에 대해서 $$R_p(x)$$ 를 정의한다. 

$$ \sum_{i=1}^p R_p = f(x)  ~~ \forall x $$

Heatmap 에 대해서 다음 조건이 성립한다. 

* $$f(x) = 0 \Rightarrow  R(x) = 0$$


## Notaiton


* Feature and Function
  * $$x_i, x_j, x_p$$ : i-th  layer feature, j-th  layer feature, pixels (scalar)
  * $$\{ x_i \}, \{ x_j \}, \{ x_p \}$$ : a set of features 
  * $$ f(x) : \mathbb{R}^p \rightarrow \mathbb{R}$$ :  a function 




* Relevance (Assume $$R_j$$ is calculated and $$R_i$$ is to be calculated )
    * $$f(x) = \sum_p R_p(x)$$ : 
    * $$R_p(x) $$ : input contribution of $$x$$ for $$f(x)$$ (scalar) 
    * $$\{ R_p(x) \}$$ : a set of relevances for all pixels.
    * $$R_{ij}$$ : propagation $$ i \rightarrow j$$ (scalar)
    * $$R_i = \sum_j R_{ij}$$  : 

* Root point
    * $$\tilde{x}_i$$  : root point for $$x_i$$ (scalar)
    * $$\{\tilde{x}_i\}$$ : root point for  $$\{x_i\}$$ (vector)
    * $$\{\tilde{x}_i\}^{(j)}$$ : root point for $$\{x_i\}$$ for the neuron at $$j$$ 


## Taylor Decomposition 


임의의 미분 가능한 함수 $$f(x)$$ 에 대해서 적절하게 찾은 Root point $$\tilde{x}$$, $$f(\tilde{x}) =0$$ 이라면 다음과 같이 first-order Taylor expansion of $$f(x)$$ 를 구할 수 있다. 

$$ f(x) = f(\tilde{x})  + \Big(  \frac{\partial f}{ \partial x} |_{(x = \tilde{x})} \Big)^{\text{T}} \cdot ( x - \tilde{x}) + \epsilon = 0 +   \frac{\partial f}{ \partial x_p} | (x = \tilde{x}) \cdot ( x_p - \tilde{x}_p) + \epsilon$$ 

위의 정의에 의해서 

$$R_p=\frac{\partial f}{\partial x_p} | (x = \tilde{x}) \cdot (x_p - \tilde{x}_p)$$ 
이 성립함을 확인할 수 있다. 


## Deep Taylor Decomposition 


$$R_j = \Big(  \frac{\partial R_j}{ \partial \{x_i\}} |_{\{ \tilde{x}_i \}^{(j)}} \Big)^{\text{T}} ( \{x_i\} -  \{\tilde{x}_i\}^{(j)}) + \epsilon_j = \sum_{i} \frac{\partial R_j}{\partial x_i} |_{\{\tilde{x}_i\}^{(j)}} \cdot (x_i - \{\tilde{x}_i\}^{(j)}) + \epsilon_j$$