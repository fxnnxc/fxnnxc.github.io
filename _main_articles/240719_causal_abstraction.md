---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: false
date: 2024-07-18
featured: true
title: 'Study on Causal Abstraction for Distributed Alignment Search.'
description: ''
_styles: >
    .table {
        padding-top:200px;
        margin-bottom:  2.5rem;
        border-bottom: 2px;
    }
    .p {
        font-size:20px;
    }
    .center{
        display: block;
        margin-left: auto;
        margin-right: auto;
        width: 50%;
    }
---


## What is Causal Abstraction?

Causal Abstraction is an approach to simplify the relationships between multiple variables in a complex system, making it easier to understand the key causal relationships. This process involves the following steps:

* Identifying Key Variables: Among many variables, identify the ones that have the most significant impact on the system.
* Setting Abstraction Levels: Abstract at a higher level by omitting details and consolidating lower-level variables into broader categories.
* Creating Simplified Models: Generate simplified models to understand and explain the complex interactions. This often focuses on understanding the causal relationships between high-level variables.
* Analysis and Prediction: Use the simplified models to analyze the system's behavior and make predictions based on the key variables' causal relationships.

This process provides several benefits:

* Ease of Understanding: Complex systems become easier to understand.
* Simplified Analysis: Analysis is streamlined by focusing on important relationships.
* Improved Explainability: The results of the model are easier to interpret and explain.
* Efficient Prediction: Predictions can be made more efficiently using the simplified model.



## Problems 

* causal abstraction requires a computationally intensive brute-force search process to find optimal alignments between the variables in the high-level model and the states of the low-level one.
* prior methods are localist: they artificially limit the space of possible alignments by presupposing that high-level causal variables will be aligned with disjoint groups of neurons.


## What is interchange intervention accuracy; IIA? 

* interchange intervention (also known as activation patching),

Interchange intervention accuracy (IIA) is a graded measure of abstraction that computes the proportion of aligned interchange interventions on the algorithm and neural network that have the same output.


## Distributed Interchange Interventions

which are “soft” interventions in which the causal mechanisms of a group of neurons are edited
such that 
(1) their values are rotated with a change-of-basis matrix. 
(2) the targeted dimensions of the rotated neural representation are fixed to be the corresponding values in the rotated neural
representation created for the source inputs. 
(3) the representation is rotated back to the standard neuron-aligned basis.


> Remark: The target dimensions is the eigenvector of the rotation matrix. 