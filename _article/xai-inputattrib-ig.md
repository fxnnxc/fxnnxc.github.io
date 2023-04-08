---
layout: post
title: 'Integrated Gradient [⚙️]'
date: 2022-05-08 00:00:00-0400
description: 'Test'
tags: Stats, Cluster
category: XAI
subcategory : InputAttrb
---

# IG

Integrated Gradient (IG) is proposed by Sundararajan et al. 



# Baselines 

The importance of baselines is introduced in [2]. 



----
Definition of meaningless is unclear in the

In practice, a baseline should be selected with respect to the data distribution at hand


### common baseline methods

(Lundberg and Lee 2017;
Sundararajan and Najmi 2019; Izzo et al. 2020; Sturmfels,
Lundberg, and Lee 2020).


Let $x_i, x_j \sim X$ be two arbitrary observation 
#### Static or Dynamic Baseline 
* A baseline method $B$ is static if $\forall i,j : b_{x_i} = b_{x_j}$. Otherwise, $B$ is dynamic
* A static baseline method provides the same baseline value for every observation 

####  Deterministic or Stochastic Baseline
*  A baseline method $B$ is deterministic, if the probability $Pr(b_{x_i}^1 = b_{x_i}^2) = 1$. Otherwise, $B$ is stochastic 
* Intuitively, a deterministic baseline method always produces the same baseline with respect to an observation $x_i$ whenever it is called. 


|----|---| ---|:---|
|Baseline | Static/Dynamic| Deterministic/Stochastic| info |
|Constant (e.g. zero baseline) [1] |static| deterministic| |
|Maximum Distance [7] | dynamic| deterministic| corresponds to an observation that is furthest away from the observation in question by the `1-norm` |
|Blurred [6,7]| dynamic| stochastic| Gaussian blur filter to the observation. This method requires the assumption of adjacent features, which may not be the case of tabluar dataset,|
|Gaussian [7,8] |dynamic| stochastic| Gaussian distribution per input feature, which is centered at the original input value and randomly sample baselines |
|Uniform [7]| dynamic| stochastic|The uniform distributions are defined in the valid range of the original features |
|Expectation [5] |static| stochastic| the expectation of a reference sample |
| Neutral [4] |static| deterministic| a baseline should lie on the decision boundary of the predictive model. |


In experiment, the authors observe that the
* `constant`, `blurred`, `Gaussian` and `expectation` baseline produce competitive results 
* whereas the `uniform` and `maximum distance` baseline tend to generate less discriminative feature attributions.



---

# References 


[1] Sundararajan, Mukund, Ankur Taly, and Qiqi Yan. "Axiomatic attribution for deep networks." International conference on machine learning. PMLR, 2017.

[2] Sturmfels, Pascal, Scott Lundberg, and Su-In Lee. "Visualizing the impact of feature attribution baselines." Distill 5.1 (2020): e22.


[3] Haug, Johannes, et al. "On baselines for local feature attributions." arXiv preprint arXiv:2101.00905 (2021).



[4] Izzo, C.; Lipani, A.; Okhrati, R.; and Medda, F. 2020. A Baseline for Shapely Values in MLPs: from Missingness to Neutrality. arXiv preprint arXiv:2006.04896


[5] Lundberg, S. M.; and Lee, S.-I. 2017. A unified approach to interpreting model predictions. In Advances in neural information processing systems, 4765–4774

[6] Fong, R. C.; and Vedaldi, A. 2017. Interpretable explanations of black boxes by meaningful perturbation. In Proceedings of the IEEE International Conference on Computer
Vision, 3429–3437

[7] Sturmfels, P.; Lundberg, S.; and Lee, S.-I. 2020. Visualizing the impact of feature attribution baselines. Distill 5(1): e22.

[8] Smilkov, D.; Thorat, N.; Kim, B.; Viegas, F.; and Wattenberg, M. 2017. Smoothgrad: removing noise by adding noise. arXiv preprint arXiv:1706.03825 .