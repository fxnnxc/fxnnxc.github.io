---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2023-09-11
featured: true
img: 
title: 'GPT Math Domain Experiment'
description: 'pretraining and finetuning GPT models with math symbols'
---

* **Code**: <a href="https://github.com/fxnnxc/bumjin_research/tree/main/bumjin_research/labs/gpt_math_domains">  <img src="/assets/github.png" style="width:1.2em;" > </a> (Private) 


## Introduction 


GPT is known to be effective for causal language modeling. 
In this work, we present training results of GPT on mathematical symbols. 
There are two types of datasets and two configurations respecrtively. 

We trained GPT models of three sizes. 




#### Models 

version | **num layers** | **num heads** | $d_{model}$ | 
|:-:|:-:|:-:|:-:|
2 | 1     |      8    |      128  |
4 | 2     |      8    |      128  |
6 | 8     |      8    |      512  |


#### Datasets 

Version |  **Digits** (0~N) | **Modulo** (2~N)
|:-:|:-:|:-:|
1 |10 | 5 
3 |20 | 5 


#### Training Configuration

<d-code block language='python'>
# training configuration
# variants = 1000 / 5000 / 10000 for model sizes respectively. 
"num_train_epochs": variants,
"eval_steps": 1_000,
"batch_size":32,
"learning_rate":5e-4,
"gradient_accumulation_steps":1,
"weight_decay":0.1,
"num_warmup_steps":1_000,
</d-code>



## Math Symbol Prediction



<div style="display:flex;justify-content:center; width:60em;margin-left:-8em;">
<iframe src="{{ '/assets/plotly/gpt_math_domain/pretrain_1_eval_math_acc.html'  relative_url }}" frameborder='0' scrolling='no' height="380px" width="100%"   style="border:1px dashed grey; padding-bottom:0px"></iframe>
<iframe src="{{ '/assets/plotly/gpt_math_domain/pretrain_3_eval_math_acc.html'  relative_url }}" frameborder='0' scrolling='no'  height="380px" width="100%" style="border:1px dashed grey; padding-bottom:0px"></iframe>
</div>


## Domain Prediction 



<div style="display:flex;justify-content:center; width:60em;margin-left:-8em;">
<iframe src="{{ '/assets/plotly/gpt_math_domain/finetune_1_pretrain_1_eval_domain_acc.html'  relative_url }}" frameborder='0' scrolling='no' height="380px" width="100%"   style="border:1px dashed grey; padding-bottom:0px"></iframe>
<iframe src="{{ '/assets/plotly/gpt_math_domain/finetune_1_pretrain_3_eval_domain_acc.html'  relative_url }}" frameborder='0' scrolling='no'  height="380px" width="100%" style="border:1px dashed grey; padding-bottom:0px"></iframe>
</div>

<br>
<br>
<br>
<br>
<br>
<br>


## Conclusion