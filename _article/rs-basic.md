---
layout: post
title: 'Basics of Recommendation Systems'
date: 2023-06-11 00:00:00-0400
description: 'Basics of Recommendation Systems'
tags: RS 
category: RS
subcategory : RS 
---


# 1.  History of Recommendation System 

1. Memory-based Model : Directly search in history and aggregate the scores.  
2. Latent Factor Model : Both users and items can be characterized by a few latent features. 
3. Representation Learning Model : Capture  the local item relationships by model item co-occurrence in each user's interactions. 
4. Deep Learning Model : high-order and non-linear latent representations


## 1.1 Memory-based Model 

* User-based CF : identify similar users and aggregate the interests of neighbors for recommendations.
* Item-based CF : find similar items ans estimate the rate. 

## 1.2 Latent Factorization Model (LFM)

$$
\hat{r}_{ui} = q_i^\top p_u, \min_{q,p} \sum_{u,i} (r_{ui} - q_i^\top p_u)^2 + \lambda (\Vert q_i \Vert^2 +\Vert p_u \Vert^2 )
$$

* As the items are mapped to minimize the loss, the latents can be understood as features of the shallow network (He et al., 2016)


## 1.3 Representation Learning Model (RLM)

Item2Vec (Barkan et al, 2016; Liang et al., 2016, Feng et al., 2018)

## 1.4 Deep Learning Model (DLM)

* RNN based approaches have shown strong capabilities for sequential recommendations
* CNN are able to extract hierarchical features of various contextual information. 
* How to incorporate the DLM features as a side information remains a promising research direction. 

# 2. Side Information 


The side information is additional information which is out of the observation framework, but related to the item. For example, in the user-item matrix of a supermarket dataset, objects description is side information and the age of a user is side information as they are not included in the observation framework. The goal of side information is to use the information so that the prediction of rating and ranking be accurate [1]. 

* Structural Data 
  * Flat Feature (FF)
  * Feature Hierarchy (FH) 
  * Network Feature (NF) 
  * Knowledge Graph  (KG)
* Non-Structural Data 
  * Text Feature (TF)
  * Image Feature  (IF)
  * Video Feature  (VF)


## 2.1 LFM with Side Information 

* Matrix Factorization (MF)
* Weighted Non-Negative Matrix Factorization (WNMF)
* Bayesian Personalized Ranking (BPR)
* Tensor Factorization (TensorF)
* Factorization Machine (FM)


# Notes 

In RS, obtaining a proper representation of user's item and user itself are crucial. 
There are two major tasks in RS, rating and ranking of items. The items must be evaluated to be recommended to the user. 
If the user is new, he must be compared with previous users with a few interaction history (collaborative filtering). If the user profile includes rich information, items are evaluated with his profile (content-based filtering). To effectively learn the recommendation system, learning a side information is crucial. However, the definition of side is formation is unclear (solved in [1]). 
