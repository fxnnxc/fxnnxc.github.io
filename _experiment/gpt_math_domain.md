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

## Introduction 

* Code (Private) [Github](https://github.com/fxnnxc/bumjin_research/tree/main/bumjin_research/labs/gpt_math_domains). 


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