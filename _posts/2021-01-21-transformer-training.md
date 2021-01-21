---
title: 'Training Transformer is not easy'
date: 2021-01-21
permalink: /posts/2021/01/transformer-training/
tags:
  - training
  - transformer
  - bart
---

# Transformer

## Comparing with other models

The transformer is really sensitive to the learning rate. It is harder to optimize the transformer than the CNN or Seq2Seq model.
* Reason : Initial gradient is so unstable
* Warmup stage comes here. but we need more hyperparameter tuning.

## Epoch and Learning rate

* Training for **too many epochs** or using **too high a learning rate** typically leads to catastrophic forgetting with the model often resorting to generating the same output/label to any given input. 
* On the other hand, an **insufficient number of training epochs** or **too low a learning rate** results in a sub-par model.

The model ended up predicting the same label for all inputs, suggesting that the learning rate is too high. Lowering the learning rate to 1e-5 was sufficient to avoid this issue.

## Parameters vs Hyperparameters
* determine what the output will be for any given input. Training a deep learging model is the process of finding the set of values for these parameters which yield the best results on the given task.
* hyperparameters are the factors which control the training process itself. The learning rate, the number of training epochs/iterations, and the batch size are some examples of common hyperparameters
* In a nutshell, the parameters are what the model learns, and the hyperparameters determine how well (or how badly) the model learns.


## References

[1] Understanding and Improving Layer Normalization
https://papers.nips.cc/paper/2019/file/2f4fe03d77724a7217006e5d16728874-Paper.pdf

[2] Hyperparameter Optimization for Optimum Transformer Models
https://towardsdatascience.com/hyperparameter-optimization-for-optimum-transformer-models-b95a32b70949

[3] Training Tips for the Transformer Model
https://arxiv.org/abs/1804.00247