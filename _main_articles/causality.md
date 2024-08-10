---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2024-08-09
featured: true
title: "Causality"
description: 'What do I know about causality?'
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
    width: 100%; 
}
</style>




## Definition 

* **Endogenous variable**: variable is determined by a structure equation, is affected by other variables.
* **Exogenous variable**: external cause, which can affect the endogenous variable, but the reverse does not hold (i.e. endogenous variable cannot affect the exogenous variable, but the reverse is possible.)


## Structure Causal Model 

<blockquote>
Consider two sets of random variables $\mathbf{X}$ and $\mathbf{U}$,a <strong>Structure Causal Model</strong> is a tuple $M:= (\mathcal{F}, P(U))$, where $\mathcal{F}$ comprises a set of $d$ structural equations $f_i$, one for each endogenous random variable, $X_i \in \mathbf{X}$, $\mathcal{F}= \{X_i := f_i (PA(X_i), U_i)\}_{i=1}^d$ where $PA(X_i) \subset \mathbf{X} \setminus \{X_i\}$,an exogenous noise variable $U_i \in \mathbf{U}$, and $P(\mathbf{U})$ is a strictly positive over the exogenous variable.   
</blockquote>

* An SCM characterizes a unique distribution over the variables $P_{\mathcal{M}}(\mathbf{X})$ which is called **entailed distribution**. 
* Conditional distribution of a variable given its parents, $$P_{\mathcal{M}}(X_i \vert PA(X_i))$$, is called a **causal kernel**. 
* **Causal Graph**: $$\mathcal{G} := (\mathcal{V}, \mathcal{E})$$, which encodes the causal dependencies between the variables. Each node is an endogenous variable, and the set of directed edges represents the parent-child relationships, i.e., $\mathcal{V} = \mathbf{X}$ and $$\mathcal{E} = \{PA(X_i) \rightarrow X_i, \forall i \in [d]\}$$.

## Intervention 

Changing at least one of the structural equations 

$$
\textit{do}(X_k := \tilde{f}_k (\tilde{PA}(X_k), \tilde{U_k}))
$$

This operation defines a new SCM $\tilde{M}$ whose entailed distribution is called the intervention distribution 

$$ 
P_{\tilde{\mathcal{M}}}(\mathbf{X}) = P_{\mathcal{M}}\Big(\mathbf{X}\vert \textit{do}(X_k:= \tilde{f}_k (\tilde{PA}(X_k), \tilde{U_k}))\Big)
$$


## Counterfactual 

Note that the endogenous variables are mostly determined after the exogenous variables. 
The counterfactual analysis assumes that the result $y$ is obtained with a causal mechanism and asks the question what would have happened if the endogenous variable $X$ had been changed while the exogenous variables are fixed. This analysis provides a deeper analysis on the treatment endogenous variable $X$. In summary, the intervention and the counterfactual have such a difference

1. **Intervention**: Change the endogenous variable $X$ by changing the structure equation. This is  mostly conducted by the intervention. 
2. **Counterfactual**: Given the result $Y=y$ and the realization of exogenous variables $U$, intervene the endogenous variable to provide a counterfactual controlled experiment. 


<blockquote>
Three-step procedure for the counterfactual reasoning. <br>

1. <strong>Abduction</strong>: Update the knowledge of the exogenous variables. <br>
2. <strong>Action</strong>: Intervene the targeted endogenous variable <br>
3. <strong>Reasoning</strong>: Get the prediction.

</blockquote>

Given an SCM $\mathcal{M}$ and some factual realization $y$ of $\mathbf{Y} \subset \mathbf{X}$, we obtain the posterior distribution of exogenous variable, $P_{U \vert Y=y}$. It defines a new SCM $\mathcal{M}^{'}$, where $$P_{\mathcal{M}^{'}}(\mathbf{U}) = P_{\mathcal{M}^{'}}(\mathbf{U} \vert \mathbf{Y}=y)$$. Counterfactual includes also an additional intervention. 

$$
P_{\mathcal{M}}^{Y=y}\Big(\mathbf{X}| \textit{do}(X_k := \tilde{f}_k (\tilde{PA}(X_k), \tilde{U_k})\Big)
$$


## Identifiability 

* **Structure Identifiability** : whether the causal graph can be recovered
* **Model Identifiability** : whether the full causal model can be recovered
* **Estimand Identifiability** : whether a specific causal estimand, such as a treatment effect, can be estimated. 
* **Intervention Identifiability**  : whether all intervention queries can be retrieved. 
* **Counterfactual Identifiability** : whether all counterfactual queries can be retrieved. 

Note that model identifiability **implies** intervention and counterfactual identifiability.

## Classes of SCM


### Bijective Generation Mechanisms

### Deep Structural Causal Models (Neural Causal Models)


## Classes of DGM 


### Invertible Explicit

### Amortised Explicit

### Amortised Implicit


---

## References 

[1] Learning Structural Causal Models through Deep Generative Models: Methods, Guarantees, and Challenges