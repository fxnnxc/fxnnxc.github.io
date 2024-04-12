---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2024-04-12
featured: true
img: 
title: 'Mamba Factual Knowledge Editing'
description: 'This article is a review of the paper:'
---



* Language Model: $M : \mathcal{X} \rightarrow \mathcal{Y}$ 
* A sequence of Tokens: $x = [x_1, x_2, \cdots, x_T] $, $x_i \in \mathcal{V}$ 



$o_i^{(\ell)}$ is the output of the block 

$$
h_i^{(\ell)} = h_i^{(\ell-1)} + o_i^{(\ell)}
$$

