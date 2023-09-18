---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2023-09-17
featured: true
title: 'Distribution'
description: 'Study of Distribution'
---


---

## Binomial distribution

The RV $X$ follows the binomial distribution with parameter $n\in \mathbb{N}$ and $p \in [0,1]$ 

$$
X \sim B(n, p)
$$

The probability of getting exactly $k$ successes in $n$ independent Bernoulli trails is given by the probability mass function. 

$$
f(k,n,p) = Pr(k;n,p) = Pr(X=k) = \begin{pmatrix} n \\ k \end{pmatrix} p^k (1-p)^{n-k}
$$

* Mean: $\mathbb{E}[X] = np$
* Variance :  $\operatorname{Var}(X) = npq = np(1-p)$ 

---