---
layout: share-distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2023-08-24
featured: true
img: /assets/experiments/gpt_sip_twitter/70m_train_ppl.png
title: 'ParchGrad Paper Structure'
description: 'Series of experiments conducted for SIP for GPT'
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

This is a full discussion on the structure of the paper ParchGrad 
 

Our contributions are as follows. 
* We are the first to propose channel pruning for convolutional layers in saliency map. 
* We theoretically analyze the impact of multiple gradients and propose variance conservation to balance gradients from multiple modules. 
* We propose class-wise channel selection methods which largely removes noise in gradients while preserving interpretability. 


### Original Draft 

