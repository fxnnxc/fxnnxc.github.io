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
title: "Hoeffding's Lemma and Inequality"
description: "Description of Hoeffding's Lemma, Inequality and Applications"
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
Let $X$ be a real-valued random variable such that $$a \le X \le b$$ almost surely. Then, for all $\lambda \in \mathbb{R}$, 

$$ 
\mathbb{E} [e^{\lambda X}] \le \exp \Big(  \lambda \mathbb{E}[X] + \frac{\lambda^2 (b-a)^2}{8} \Big)
$$ 

</blockquote>
<blockquote>
Proof

Without loss of generality, we replace $X$ by $X- \mathbb{E}[X]$, and we can assume $\mathbb{E}[X]$, so that $a \le 0 \le b$.
Since $e^{\lambda x}$ is convex function of $x$, by Jensen's inequality, we have that for all $x \in [a,b]$,

$$
e^{\lambda X} \le \frac{b-x}{b-a}e^{\lambda a} + \frac{x-a}{b-a}e^{\lambda b}
$$

So, 
$$
\begin{align}
\mathbb{E}[e^{\lambda X}] &\le \frac{b-\mathbb{E}[x]}{b-a}e^{\lambda a} + \frac{\mathbb{E}[x]-a}{b-a}e^{\lambda b} \\
&= \frac{b}{b-a}e^{\lambda a} + \frac{-a}{b-a}e^{\lambda b} \\
&= e^{f(\lambda (b-a))}
\end{align}
$$

where $f(h) = \frac{ha}{b-a} + \ln (1 + \frac{a - e^h a}{b-a})$. 

By computing derivates, we find

$$
f(0) = f'(0) = 0 
$$

and 

$$
f''(h) = - \frac{ab e^h}{(b-a e^h)^2}
$$

With replacement $A = ae^h$ and $B=b$, we have 

$$
\begin{align}
\frac{ab e^h}{(b-a e^h)^2} =& - \frac{AB}{(B-A)^2}  \\
 =& \frac{AB}{B^2 -2AB + B^2} \\
 \le& \frac{AB}{4AB}  ~~~ (\because \text{AM-GM inequality}* ) \\
 =& \frac{1}{4} \\
\end{align}
$$

* <a href="/main_articles/geometry" > AM-GM inequality </a>


We see that $$f''(h) \le 1/4$$ for all $h$, thus, from <a href="/main_articles/taylor_s_theorem" > Taylor's theorem </a>, there is some $0 \le \theta \le 1$ such that 

$$
f(x) = f(0) + f'(0)x + \frac{1}{2}h^2 f''(h\theta) \le \frac{1}{8}h^2
$$

Thus, 
$$
\mathbb{E}[e^{\lambda X}] \le e^{f(\lambda (b-a))} \le e^{\frac{1}{8} \lambda^2(b-a)^2}
$$

</blockquote>







## Hoeffding's Inequality 




## Application