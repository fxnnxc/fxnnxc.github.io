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

## Causal Estimation of Memorization Profiles 

Some definitions in the work (Lesci et al., 2024), which is achieved ACL 2024 best paper. I track the definitions related to causality. 

This work defines **memorization** as "the causal effect of observing an instance during training on a model's ability to correctly predict that instance". As such the authors use the log likelihood as a performance measure of an instance $x$. **Memorization profile** is a model's memorization of training batches over the course of training. 

<blockquote markdown="1">
Given dataset $\mathcal{D} = \{ x_n \}_{n=1}^N$ whose instances $x$ are sequences drawn from a target distribution $p(x)$, at each iteration $T$, we obtain a new model checkpoint $\mathbf{\theta}_t$ with batch $\mathcal{B}_t$. Let $g \in \{ 1,2, \cdots, T \} \cup \{ \infty \} $ be the timestep. The authors denote $g = \infty$ a batch composed of instances that are not used for the training and which form a validation set. 
<br>
The act of training on $x$ defines **the treatment**, while the model's ability to predict an instance defines **the outcome.** The authors define 
* Treatment assignment variable $G(x)$ to denote the step $g$ an instance is trained on. 
* Outcome variable $Y_c(x) = \gamma (\theta_c, x) = \log p_{\theta_c} (x)$ 
</blockquote>

<blockquote markdown="1">
**Definition 1**: The **potential outcome** of an instance $x$ at timestep $x$ under treatment assignment $g$, denoted as $Y_c(x;g)$, is the value that the outcome would have taken if $G(x)$ was equal to $g$. 

Note: this evaluate the $x$ at $c$ when $x$ was trained at $g$. 

**Definition 2**: **Counterfactual memorization** is the causal effect of using instance $x$ for training at the observed timestep $G(x)=g$ on the model's performance on this same instance at timestep $c$. 

$$
\tau_{x,c} = Y_c(x;g) - Y_c(x; \infty)
$$

Note: this evaluate the $x$ at $c$ when (1) $x$ was trained at $g$ and (2) is never used in the training. 


</blockquote>


<blockquote markdown="1">
<h2> Assumptions </h2>

**Assumption 1** : Instances $x$ are independently and identically distributed, following $p(x)$, and are randomly assigned to treatment group $g$. 

Under this assumption, we have

$$
p(x|G(x) = g) = p(x|G(x) = \infty) =  p(x)
$$

**Assumption 2**: In the absence of training, the expected change in model performance across checkpoints would be the same regardless of treatment. That is, for all $c,c' \ge g -1$, 

$$
\mathbb{E}_x [Y_c(x;\infty) - Y_{c'}(x;\infty) \vert G(x) = g] = 
\mathbb{E}_x [Y_c(x;\infty) - Y_{c'}(x;\infty) \vert G(x) = \infty]
$$

**Assumption 3**: Training has no effect before it happens. That is, for all $c < g$:

$$
\mathbb{E}_x [Y_c(x; g) \vert G(x) = g] = \mathbb{E}_x [Y_c(x;\infty) \vert G(x) = g]
$$

</blockquote>


<blockquote markdown="1">
<h2> Estimators </h2>
**Definition 3**: **Expected Counterfactual memorization** is the average causal effect of using instances for training at timestep $g$ on the model's performance on these same instances at timestep $c$:

$$
\tau_{g,c} = \mathbb{E}_{x} \Big[ Y_c(x;g) - Y_c(x; \infty)  \vert G(x) = g \Big]
$$

**Estimator 1** : The **difference estimator**, defined as: 

$$
\hat{\tau}_{g,c}^{\operatorname{diff}} = \bar{Y}_c(g) - \bar{Y}_c({\infty})
$$

**E1** is an unbiased estimator of $\tau_{g,c}$ under Assumption 1. Usually, the meaning of unbiased is 
$$
\mathbb{E}[\hat{\tau}] = \frac{1}{K} \sum_{i=1}^K \hat{\tau}_i = \tau
$$

but, in this setting, the average is over the instances:

$$
\mathbb{E}[\hat{\tau}] = \mathbb{E}_{\mathcal{B}_g, \mathcal{B}_\infty} [\hat{\tau}_{g,c}] = \tau
$$

**Estimator 2** : The **difference-in-differences estimator** (DiD), defined as:

$$
\hat{\tau}_{g,c}^{\operatorname{did}} = 
\Big( \bar{Y}_c(g) - \bar{Y}_{g-1}(g)  \Big)
- \Big( \bar{Y}_c(\infty) - \bar{Y}_{g-1}(\infty) \Big) 
$$

where $$ \bar{Y}_c(g) = \frac{1}{\vert \mathcal{B}_g \vert} \sum_{x\in \mathcal{B}_g} Y_c(x) $$

</blockquote>


---



## References 

[1] Learning Structural Causal Models through Deep Generative Models: Methods, Guarantees, and Challenges