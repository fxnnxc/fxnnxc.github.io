---
layout: post
title: 'Transductive Learning'
date: 2022-12-20 11:12:00-0400
description: transductive learning methods
tags: RepresentationLearning
category: DL
---

## Introduction


<blockquote>
Transduction was introduced by Vladimir Vapnik in the 1990s, motivated by his view that transduction is preferable to induction since, according to him, induction requires solving a more general problem (inferring a function) before solving a more specific problem (computing outputs for new cases): "When solving a problem of interest, do not solve a more general problem as an intermediate step [wiki, 2]
</blockquote>


### Transductive SVM


Train a SVM classifier with additional unlabeled test data points. 
This helps to move the decision boundary to low local density region. 


### Transductive Few-shot learning 

* inductive : given input, produce a label correspondig to it.
* transductive : model can access all query data point that we want to classfity. [1]

In practive, we use additional samples in the training data to make a better embedding space, such as label propgation and embedding propagation.


**Advantange**

* shows better results when training points are sparse.  

**Disadvantage**

* when we data point is added, we should retrain the model, and the prediction may be different. 


## References

[1] https://opencv.org/understanding-transductive-few-shot-learning/

[2] https://en.wikipedia.org/wiki/Transduction_(machine_learning)