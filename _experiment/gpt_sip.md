---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2023-08-21
featured: true
img: 
title: '[Experiment Note] SIP for GPT'
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

### 2023.08.23 

|Paper | Summary | 
|:--|:--|
[Prefix-Tuning](https://aclanthology.org/2021.acl-long.353/)  | train a tokens while freezing the parameters
[P-Tuning v2](https://arxiv.org/pdf/2110.07602.pdf) | P-Tuning is not adequate for small-size models. <br> P-Tuning V2 is an implementation of Deep Prompt Tuning 
|[GPT Understands, Too](https://arxiv.org/abs/2103.10385)| P-tuning employs trainable continuous prompt embeddings|
[Prompt Tuning](https://arxiv.org/abs/2104.08691)| ??

* [Todo in PEFT](https://github.com/huggingface/peft/blob/main/examples/causal_language_modeling/peft_prefix_tuning_clm.ipynb)



