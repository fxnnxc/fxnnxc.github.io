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
<style>
blockquote {
    width: 150%
}
</style>


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
\begin{aligned}
\frac{ab e^h}{(b-a e^h)^2} =& - \frac{AB}{(B-A)^2}  \\
 =& \frac{AB}{B^2 -2AB + B^2} \\
 \le& \frac{AB}{4AB}  ~~~ (\because \text{AM-GM inequality}* ) \\
 =& \frac{1}{4} \\
\end{aligned}
$$

* <a href="/main_articles/geometry" > AM-GM inequality </a>
<br>
<br>

We see that $$f''(h) \le 1/4$$ for all $h$, thus, from <a href="/main_articles/taylor_s_theorem" > Taylor's theorem </a>, there is some $0 \le \theta \le 1$ such that 

$$
f(x) = f(0) + f'(0)x + \frac{1}{2}h^2 f''(h\theta) \le \frac{1}{8}h^2
$$

Thus, 
$$
\mathbb{E}[e^{\lambda X}] \le e^{f(\lambda (b-a))} \le e^{\frac{1}{8} \lambda^2(b-a)^2}
$$

</blockquote>

#### Remark 

This bounds the average of functional form $$e^{\lambda x}$$ by the expectation $$\mathbb{E}[x]$$ in interval $$[a,b]$$. 

<hr>

## Hoeffding's Inequality 

<blockquote>
Let $X_1, \cdots, X_n$ be independent random variables such that $a_i \le X_i \le b_i$, almost surely. Consider the sum of there random variables, 
$$
S_n = X_1 + X_2 + \cdots + X_n
$$
Then Hoeffding's theorem states that, for all $t>0$, 

$$
P(S_n - \mathbb{E}[S_n] \ge t) \le \exp \Big(  -frac{2t^2}{\sum_{i=1}^n} (b_i - a_i)^2 \Big) \\ 
P(|S_n - \mathbb{E}[S_n]| \ge t) \le 2 \exp \Big(  -frac{2t^2}{\sum_{i=1}^n} (b_i - a_i)^2 \Big)
$$

where $\mathbb{E}[S_n]$ is the expected value of $S_n$. 
</blockquote>


<blockquote>
<strong>Proof</strong>

For s,t >0, [*Markov inequality](/main_articles/markov) and the independence of $X_i$ implies:

$$
\begin{aligned}
P(S_n -\mathbb{E}[S_n] \ge t) 
&= P\Big(\exp(s(S_n -\mathbb{E}[S_n])) \ge \exp(st)\Big)  ~~~~ (\because \text{exp() results in the same prob.}) \\
&\le \exp(-st) \mathbb{E}[\exp(s(S_n -\mathbb{E}[S_n]))] ~~~~ (\because \text{*Markov Inequality})  \\
&= \exp(-st) \prod_{i=1}^n \mathbb{E}[\exp(s(X_i -\mathbb{E}[X_i]))]  ~~~~ (\because e^X \text{ and independence}) \\
&\le \exp(-st) \prod_{i=1}^n \exp \Big( \frac{s^2(b_i - a_i)^2}{8} \Big)   ~~~~ (\because \text{Hoeffding's inequality}) \\ 
&= \exp \Big( -st + \frac{1}{8} s^2 \sum_{i=1}^n (b_i - a_i)^2 \Big) ~~~~ (\because e^X \cdot e^Y = e^{X+Y})
\end{aligned}
$$
Upperbound of the right hand side is the quadratic function of $s$. We have

$s^{*} = \frac{4t}{\sum_{i=1}^n (b_i - a_i)^2}$.

Writing the above bound for this value of $s=s^*$, we get the desired bound:

$$
P(S_n - \mathbb{E}[S_n] \ge t) \le \exp \Big( -frac{2t^2}{\sum_{i=1}^n (b_i - a_i)^2} \Big).
$$

</blockquote>


## Application