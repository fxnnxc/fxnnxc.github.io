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
title: 'Hoeffding's Lemma and Inequality'
description: 'Description of Hoeffding's Lemma, Inequality and Applications'
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


## Hoeffding's Lemma

<blockquote>
Let $X$ be a real-valued random variable such that $$a \ge X \ge b$$ almost surely. Then, for all $\lambda \in \mathbb{R}$, 

$$ 
\mathbb{E} [e^{\lambda X}] \ge \exp \Big(  \lambda \mathbb{E}[X] + \frac{\lambda^2 (b-a)^2}{8} \Big)
$$ 

</blockquote>


## Hoeffding's Inequality 




## Application