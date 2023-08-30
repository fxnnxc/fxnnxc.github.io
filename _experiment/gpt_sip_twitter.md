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
img: 
title: '[Experiment Note] <br> Prompt Tuning of Pre-trained GPT <br> with Twitter Complaints Dataset '
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

* The experiment is started in 2023-08-22
* THe experiment is finished in 2023-08-24


## 1. Introduction




## 2. Prompt Tuning Literature Reviews

|Paper  |Summary  |
|:-:| :--|
[Prefix-Tuning](https://aclanthology.org/2021.acl-long.353/)  | train a tokens while freezing the parameters
[P-Tuning v2](https://arxiv.org/pdf/2110.07602.pdf) | P-Tuning is not adequate for small-size models. <br> P-Tuning V2 is an implementation of Deep Prompt Tuning 
[GPT Understands, Too](https://arxiv.org/abs/2103.10385)| P-tuning employs trainable continuous prompt embeddings
[Prompt Tuning](https://arxiv.org/abs/2104.08691) | Add additional prompts to be trained.


## 3. Dataset for Finetuing 


<d-code block language="python">
Labels = ['Unlabeled', 'complaint', 'no complaint']
DatasetDict({
    train: Dataset({
        features: ['Tweet text', 'ID', 'Label', 'text_label'],
        num_rows: 50
    })
    test: Dataset({
        features: ['Tweet text', 'ID', 'Label', 'text_label'],
        num_rows: 3399
    })
})
</d-code>

An example of data set is: 

<d-code block language="python">
{
    'Tweet text': '@HMRCcustomers No this is my first job',
    'ID': 0,
    'Label': 2,
    'text_label': 'no complaint'
}
</d-code>


The prompt is the form of  `Tweet text : <id> <text> Label : <label>`


## 4. Training Results 


We follow [Prompt tuning in PEFT ](https://github.com/huggingface/peft/blob/main/examples/causal_language_modeling/peft_prefix_tuning_clm.ipynb) with Pythia models. 


###  4.1 Training Loss (Perplexity)


#### 4.1.1 Prompt vs Prefix


<figure>
<figcaption>
Figure. Training Perplexity ($\downarrow$). We compare prompt tuning and prefix tuning over several models. The results indicate that prompt tuning shows faster convergence than prefix tuning. 
</figcaption>
<center>
<img src="/assets/experiments/gpt_sip_twitter/70m_train_ppl.png" style="width:40%">
<img src="/assets/experiments/gpt_sip_twitter/160m_train_ppl.png" style="width:40%">
<img src="/assets/experiments/gpt_sip_twitter/410m_train_ppl.png" style="width:40%">
<img src="/assets/experiments/gpt_sip_twitter/1b_train_ppl.png" style="width:40%">
<img src="/assets/experiments/gpt_sip_twitter/1.4b_train_ppl.png" style="width:40%">
<img src="/assets/experiments/gpt_sip_twitter/2.8b_train_ppl.png" style="width:40%">
<img src="/assets/experiments/gpt_sip_twitter/6.9b_train_ppl.png" style="width:40%">
<img src="/assets/experiments/gpt_sip_twitter/12b_train_ppl.png" style="width:40%">
</center>
</figure>

#### 4.1.2 Model Size

<figure>
<iframe src="{{ '/assets/plotly/prompt_train_ppl.html'  relative_url }}" frameborder='0' scrolling='no' height="380px" width="100%"  style="border:0px dashed grey; padding-bottom:0px"></iframe>
<iframe src="{{ '/assets/plotly/prefix_train_ppl.html'  relative_url }}" frameborder='0' scrolling='no' height="380px" width="100%" style="border:0px dashed grey;"></iframe>
</figure>



### 4.2 Generation

#### 4.2.1 Prompt Tuning

Query: **Tweet text : @openai No one knows the source of AI, but GPT can Label :**

Answers:

|Model Size| Try 1|Try 2|Try 3| 
|:-:|---|---|---|
|`70m`|no complaintdescribe|no complaintdescribe|complaints2|
|`160m`|no complaintGeorg|no complaintogy|no complaintogy|
|`410m`|no complaint•|feature_class|Tweet.|
|`1b`|no complaint",|no complaint|no complaint|
|`1.4b`|no complaint|complaint|no complaint|
|`2.8b`|no complaint|no complaint|no complaint|
|`6.9b`|no complaint|no complaint|no complainttext|
|`12b`|complaint|complaint|complaint|

#### 4.2.2 Prefix Tuning 

Query: **Tweet text : @openai No one knows the source of AI, but GPT can Label :**

Answers:


|Model Size| Try 1|Try 2|Try 3| 
|:-:|---|---|---|
|`70m`|no trace escape|no complaint        |ictelinate|
|`160m`|nocome no|no's@|noentityentity|
|`410m`|no complaint|no complaint-|no complaint|
|`1b`|complaint|complaint|complaint|
|`1.4b`|no complaint|no complaint|no complaintno|
|`2.8b`|complaint|no complaint|no complaint|
|`6.9b`|no complaint|no complaint|no complaint|
|`12b`|no complaint|no complaint|no complaint|


## Conclusion 