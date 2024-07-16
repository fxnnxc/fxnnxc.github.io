---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: false
date: 2024-07-15
featured: true
title: 'Review on "What needs to go right for an induction head?
A mechanistic study of in-context learning circuits and their formation"'
description: 'The emergence of induction heads in training GPT.'
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




### The field of optogenetics in neuroscience

This field combines **genetic** and **optical** methods to precisely target and manipulate specific neurons within the brain, enabling detailed studies of neural circuits and brain functions.

By selectively activating or inhibiting specific neurons, researchers can map the connections and functions of different neural circuits, understanding how they contribute to behavior and cognition.

Optogenetics allows for the real-time control of behavior by manipulating neural circuits. This has been used to study processes such as learning, memory, fear, reward, and social behaviors.

### What is Clamping? 
clamping (causal manipulations of activations throughout training)

### “induction strength” of a head
A difference in attention weights from the query token

For two label tokens, measure the attention difference. 
$$ 
A_{correct} - A_{incorrect} 
$$ 

For example, "A 0 B 1 A" case will have 

$A_{5,2} - A_{5,4}$

### To verify that Layer 2 induction heads instead of Layer 1 heads are primarily contributing to task performance

At the position of the last token, the previous token activations in the first layer can be ablated by zero attention weight and zero values.
"A 0 B 1 A"


Specifically, we note that different Layer 2 heads exhibit phase changes at
different times (but all eventually learn to solve the task).
Head 3 is the “quickest to learn” in Figure 4b, which may
also be the reason it becomes the strongest when training
with all heads (Figure 3b)—perhaps heads are racing to
minimize the loss, similar to the mechanism in Saxe et al.
(2022).

Once Head 3 emerges, though, the ordering of phase
changes in Figure 4b does not match emergence in the full
network


Reverse-S phase changes in the loss are often due to multiple interacting components