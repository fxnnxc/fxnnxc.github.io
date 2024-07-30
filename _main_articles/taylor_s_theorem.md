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
title: "Taylor's Theorem"
description: 'Description of Taylor's Theorem and Applications'
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

<blockquote>
<strong> Taylor's Theorem </strong>
Let $k \ge 1$ be an integer and let the function $f: \mathbf{R} \rightarrow \mathbf{R}$ be $k$ times differentiable at the point $a \in \mathbf{R}$. Then there exists a function $h_k: \mathbf{R} \rightarrow \mathbf{R}$ such that

$$
f(x) = f(a) + f'(a)(x-a) + \frac{f^{''}(a)}{2!}(x-a)^2 + \cdots + \frac{f^{(k)}(a)}{k!}(x-a)^k + h_k(x) (x-a)^k,
$$
and 
$$
\lim_{x\rightarrow a} h_k(x) = 0
$$

</blockquote>

* **Note**: $p_n(x) = f(a) + f'(a)(x-a) + \frac{f^{''}(a)}{2!}(x-a)^2 + \cdots + \frac{f^{(k)}(a)}{k!}(x-a)^k + h_k(x) (x-a)^k$ 


<blockquote>
<strong> Mean-value forms of the reminder </strong>

Let $f: \mathbf{R} \rightarrow \mathbf{R}$ be $k+1$ times differentiable on the open interval with $f^{(k)}$ continuous on the closed interval between $a$ and $x$. Then

$$
R_k(x) = \frac{f^{(k+1)} (c)}{(k+1)!} (x-a)^{k+1}
$$
for some $c$ between $a$ and $x$. 

Moreover, if there exists a real number $M$ such that $f^{(k+1)} \le M$ for all $x$, then 

$$
R_k(x) \le \frac{M}{(k+1)!} \vert x-a\vert^{k+1}
$$
</blockquote>

* **Note** : This means that we can bound the magnitude of residual. See the Hoeffding's lemma. 


<blockquote>
<strong> Proof </strong>
Le $$g(t)$$ be the difference between $$(f - p) - R_n$$  where $$p$$ is $$k$$-th-order Taylor polynomial of $$f(x)$$ at $$x=t$$.  

$$
g(t) = f(x) - (f(t) + f'(t)(x-t) + \frac{f^{(2)}(t)}{2!}(x-t)^2 + \cdots + \frac{f^{(k)}(t)}{k!}(x-t)^k - R_k(x) \frac{(x-t)^{k+1}}{(x-a)^{k+1}}
$$

We have 
1. $$g(x) = f(x) - f(x) + 0 +\cdots  + 0  = 0 $$
2. $$g(a) = f(x) - (f(a) + f'(t)(x-a) + \frac{f^{(2)}(t)}{2!}(x-a)^2 + \cdots + \frac{f^{(k)}(t)}{k!}(x-a)^k - R_k(x) \frac{(x-a)^{k+1}}{(x-a)^{k+1}} $$. Then $g(a) = f(x) - p_k(x) - R_k(x) = 0$ 

As $g(t)$ satisfies Rolle's theorem, there exists $c$ between $a$ and $x$ such that $g'(c)=0$. With some derivations, we have
$$
R_k(x) = \frac{f^{k+1}(c)}{(k+1)!} (x-a)^{k+1}
$$.
</blockquote>