---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2024-01-10
featured: true
title: "Markov Related Theorems"
description: "Description of Markov Related Theorems"
_styles: >
    .table {
        padding-top:200px;
        margin-bottom: 2.5rem;
        border-bottom: 2px;
    }
    .p {
        font-size:20px;
    }

---

## Markov Inequality 

<blockquote>

If $X$ is a nonnegative random variable and $a>0$, then the probability that $X$ is at least $a$ is at most the expectation of $X$ divided by $a$:

$$
P(X\ge a) \le \frac{\mathbb{E}[X]}{a}
$$
</blockquote>

#### Intuition 

Expectation can be decomposed into two parts 
$\mathbb{E} = \Big( P(X<a) \cdot \mathbb{E}[X|X<a] \Big) + \Big( P(X\ge a) \cdot \mathbb{E}[X|X \ge a] \Big) $ 
As $\mathbb{E}[X|X \ge a]$ is larger than or equal to $a$, hence intuitively, 

$$
\begin{aligned}
\mathbb{E}(X) 
&\ge \Big( P(X\ge a) \cdot \mathbb{E}[X|X \ge a] \Big) \\ 
&\ge  P(X\ge a) \cdot a 
\end{aligned}
$$
and 
$$
\Rightarrow P(X \ge a) \le \frac{\mathbb{E}(X)}{a}.
$$