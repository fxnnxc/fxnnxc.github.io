---
layout: default
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-04-19
featured: true
title: 'Causal Variables and Distributed Neural Representations'
description: ''
---




## interchange intervention (also known as activation patching)

in which a neural network is provided a ‘base’ input, and sets of neurons are forced to take on the values they would have if different ‘source’ inputs were processed


## interchange intervention accuracy; IIA


## distributed interchange interventions, 
which are “soft” interventions in which the causal mechanisms of a group of neurons are edited such that 

(1) their values are rotated with a change-of-basis matrix, 

(2) the targeted dimensions of the rotated neural representation are fixed to be the corresponding values in the rotated neural representation created for the source inputs

(3) the representation is rotated back to the standard neuron-aligned basis.


## Atheory of causal abstraction 

specifies exactly when a ‘high-level causal model’ can be seen as an abstract characterization of some ‘low-level causal model’

The basic idea is that high-level variables are associated with (potentially overlapping) sets of low-level variables that summarize their causal mechanisms with respect to a set of hard or soft interventions


## Causal Models 

* $\mathcal{B}$ : inputs and outputs are boolean (T or F)
* $\mathcal{N}$ : a linear feed-forward neural network that solves the task. 

* $I \leftarrow i $: a setting $i$ of variable $I$. 

$$
\mathcal{B}_{V_{1} \leftarrow T}([F,T]) \\
\mathcal{N}_{h_{1} \leftarrow 1.34}([0,1 ]) \\
$$

## Alignment between two models 

$$ 
\Prod = ( \{ \Prod_X \}_X, \{ \tau_X \}_X )
$$

* high-level variables $X$ 
* low-level variables $\Prod_X$ 
* mapping from low-level variables to the high-level variables $\tau_X$  (e.g., $\tau_{V_1}(1) = T$)