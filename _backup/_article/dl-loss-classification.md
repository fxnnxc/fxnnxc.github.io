---
layout: post
title: 'Classification Losses'
date: 2023-04-04 11:12:00-0400
description: ''
tags: RepresentationLearning
category: DL
subcategory : Loss
---

## Introduction






### Class imbalance Loss 




Lin et al. [1] introduced Focal loss to minimize the loss impact from easily classified samples as such the magnitude of loss is non-trivial. 
The probability of target $p_t$.  
The original cross entropy loss is 

$$
\mathrm{CE}(p,y) = \mathrm{CE}(p_t) = - \log(p_t)
$$

Weighting factor $\alpha_t$ is set by inverse class frequency or as a hyperparameter to set by cross validation. $\alpha$-balanced CE loss is 

$$
\large{\mathrm{FL}(p_t) = - \alpha_t \log(p_t)
}
$$

### Focal Loss

Focusing parameter $\gamma \ge 0$  and modular factor $(1-p_t)^\gamma$.

$$
\large{\mathrm{FL}(p_t) = - \alpha_t (1-p_t)^\gamma \log (p_t)
}
$$

when 
* an example is misclassified and $p_t$ is small, the `modulating factor` is near 1 and the loss is unaffected. 
* As $p_t \mapsto 1$, the `factor` goes to $0$ and the loss for well-classified examples is down-weighted. 



# References 


[1] Lin, Tsung-Yi, et al. "Focal loss for dense object detection." Proceedings of the IEEE international conference on computer vision. 2017.