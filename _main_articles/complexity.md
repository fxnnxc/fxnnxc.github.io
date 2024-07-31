---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2024-07-31
featured: true
title: "Complexity"
description: 'Collection of Theorems related to Complexity'
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
<style>
blockquote {
    width: 150%; 
}
</style>


## Rademacher Random Variable 

Rademacher random variable is 

$$
\sigma_i \in \{ 1, -1 \}
$$

where $i$ is the sampling index. 

## Empirical Rademacher Complexity

Given function class  $\mathcal{F} =\{ f_1, f_2, \cdots, \}$ where $f_i$ is a function for all $i$, data samples $\mathcal{S} = \{s_1, s_2, \cdots, s_n \}$, 
the empirical Rademacher complexity of function class $\mathcal{F}$ is 

$$
\begin{equation}
\hat{R}_{\mathcal{S}} (\mathcal{F}) = \mathbb{E}_\sigma \Big[  
    \operatorname{sup}_{f \in \mathcal{F}} \frac{1}{n} \sum_{i=1}^n \sigma_i f(x_i)
    \Big]
\end{equation}
$$

## Generalization Bound 

Given a sample $$ S = \{(x_1, y_1), (x_2, y_2), \ldots, (x_n, y_n)\} $$ of $$n$$ i.i.d. 
examples from some unknown distribution, and a class of functions $$ \mathcal{F} \) mapping \( X \) to \( Y $$, the generalization bound of $f \in \mathcal{F}$ is given by:


$$
\begin{equation}
R(f) \leq \hat{R}(f) + 2 \hat{\mathcal{R}}_S(\mathcal{F}) + \sqrt{\frac{\ln(2/\delta)}{2n}}
\end{equation}
$$

where:

* $$R(f)$$ is the true risk of $f$. $$R(f) = \mathbb{E}_{(x, y) \sim \mathcal{D}} [L(f(x), y)]$$ where $L$ is a loss, e.g., CE loss. 
* $$\hat{R}(f)$$ is the empirical risk of $f$. $$\hat{R}(f) = \frac{1}{n} \sum_{i=1}^n L(f(x_i), y_i)$$.
* $\delta$ is a confidence parameter. (The bound is loss when the confidence is low, e.g. $\delta=0.1$ gives you a loose bound.)
* $n$ is the number of samples 
* $\hat{\mathcal{R}}_S(\mathcal{F})$ is the empirical Rademacher complexity of the function class $\mathcal{F}$ with respect to the sample $\mathcal{S}$ (Equation 1).


## How to interpret generalization bound

Consider you have few number of samples. You trained your model and tuned the parameters, resulting in function $f_\theta$.  
As your model is trained, the empirical risk $$\hat{R}(f_\theta) \approx 0$$ for the samples. That is, 

$$
\hat{R}(f_\theta) = \frac{1}{n} \sum_{i=1}^n L(f_\theta(x_i), y_i) \approx 0
$$

However, you are not sure whether your model $f_\theta$ will give a low risk for the true distribution $D$. That is, 

$$
R(f) = \mathbb{E}_{(x, y) \sim \mathcal{D}} [L(f(x), y)  \stackrel{?}{\approx} 0
$$

With the generalization bound, you have 

$$
\begin{aligned}
R(f_\theta) &\leq \textcolor{blue}{\hat{R}(f_\theta)} + 2 \hat{\mathcal{R}}_S(\mathcal{F}) + \sqrt{\frac{\ln(2/\delta)}{2n}} \\
& \Rightarrow 
R(f_\theta) \lesssim \textcolor{blue}{0}+ 2 \hat{\mathcal{R}}_S(\mathcal{F}) + \sqrt{\frac{\ln(2/\delta)}{2n}} \\
\end{aligned}
$$

When $n$ is large, the true risk $R(f)$ is closer to $\textcolor{blue}{0}$, meaning your model $f_\theta$ will have very low risk (close to zero). 

Note that when (1) the confidence is low (you give more uncertainty) or (2) the function class is complex (more search space), 
the generalization bound becomes loose. 


## What is the meaning of low true risk? 

Your model is trained on selective samples $$\mathcal{S} = \{ s_1, s_2, \cdots, s_n \}$$. Therefore, you can not confident on the performance 
of the trained model $f_\theta$ for true data $\mathcal{D}$ 
(we assume the set size as $\vert \mathcal{S} \vert << \vert \mathcal{D} \vert$ and the relation as $\mathcal{S}  \subset \mathcal{D} $ ). 

When 

(1) **the generalization bound is tight** and 

(2) **your empirical risk is close to zero**, 

the true risk has more chance to be close to zero. 

This low true risk means, **model $f_\theta$ will provide the expected behavior on true data $\mathcal{D}$.**