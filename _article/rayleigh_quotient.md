---
layout: post
title: 'Rayleigh Quotient'
date: 2022-12-18 12:12:00-0400
description: linear algebra
tags: linear_algebra
categories: Math
---

## Why it is important?

In [1], the authors show the relationship between PCA and CCA with Generalized Eigenproblem with Rayleigh Quotient
In detail, the problem of finding the extreme points (gradient zero) of Rayleigh Quotients reduces to the the generalized eigenproblem 



## Generalized Eigenproblem

Consider two matrices $A$ and $B$. The generalized eigenproblem is defined

$$
Aw = \lambda Bw
$$


##  Rayleigh Quotient (generalized)

$$
r = \frac{w^\mathrm{T}A w}{w^\mathrm{T}B w}
$$


## Rayleigh Quotient (single matrix)

$$
R(M, x) = \frac{x^\mathrm{T} M x }{x^\mathrm{T} M x}
$$

* $R(M, x) \in [\lambda_{min}, \lambda_{max}]$ where each of them is smallest and largest eigenvalues of $M$ respectively. 


--- 

[1] A Unified Approach to PCA, PLS, MLR and CCA, Borga et al. 