---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-07-31
featured: true
title: 'ML'
description: 'Personal Study of ML'
---



## Hello



## False Positive Rate 



<div markdown="1">

FPR is false alarm rate which is the fraction of false positives over negative samples. 



$$
FPR = \frac{FP}{TN + FP}
$$

$$FP$$ : the number of false positives, $TN$ is the number of true negatives. 

* Specificity =  1 - FPR 

<Blockquote markdown="1">
**Example 1** <d-cite key="jiang2023evading"/> <br>

For bitwise accuracy $BA(w_1, w_2)\sim B(n, 0.5)/n $, original image $I_0$, watermark $w$, and decoder $D$, FPR that the detector predicts wrongly it as AI-generated image is 

Suppose $BA(D(I_0, w) = \frac{m}{n}$ for an original image $I_0$, where $n$ is the length of the watermark and $m$ is the number of matched bits between $D(I_0)$ and $w$. The key idea is that the service provider should pick the ground-truth watermark $w$ uniformly at random. Thus, $m$ is a random variable and follows a binomial distribution $B(n, 0.5)$. 

$$
\begin{aligned}
FPR_{single} &=  Pr(BA(D(I_0), w) > \tau)   \\ 
    &=  Pr(m>n\tau) = \frac{n}{k= \lceil n\tau \rceil} \begin{pmatrix} n \\ k \end{pmatrix} \frac{1}{2^{n}}
\end{aligned}
$$


To make $FPR_{single}(\tau) <  \eta$ , $\tau$ should be at least 

$$\tau^* = \arg \min_\tau \sum_{k= \lceil n\tau \rceil}^n \begin{pmatrix} n \\ k \end{pmatrix} \frac{1}{2^{n}} < \eta$$ 

랜덤하게 뽑았을 때, 정답 watermark에서 랜덤하게 걸리므로 워터마크 예측 테스트의 성능은 그다지 좋지 않다. 
그런데, 너무 성능이 높게 나온다면, False Alarm 이라고 고려하는 것이다. 반대로 Adversarial 하게 너무 성능이 낮게 나온다면, 이 경우도 False 이다. 

$$
\begin{aligned}
FPR_{double} &=  Pr(BA(D(I_0), w) > \tau \operatorname{or} BA(D(I_0), w) < 1- \tau)   \\ 
    &=  Pr(m>n\tau) = \frac{n}{k= \lceil n\tau \rceil} \begin{pmatrix} n \\ k \end{pmatrix} \frac{1}{2^{n}}
\end{aligned}
$$

* AI 생성물에 대해서 Watermark 적용 -> Detection -> 랜덤한 정답과 유사도 비교. 

</Blockquote>

</div>

## True Positive Rate

The probability of a positive test result conditioned on truly being positive. Also called **sensitivity** 


$$ 
TNR = \frac{TN}{TN + FP}
$$ 

## True Negative Rate 


The probability of a negative test result conditioned on truly being negative. Also called **specificity**. 


$$ 
TNR = \frac{TN}{TN + FP}
$$ 