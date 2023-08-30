---
layout: distill
title: 'Probing Neurons with CLIP Concepts'
date: 2023-08-29
description: pipeline for probing
img: 'https://drive.google.com/uc?export=view&id=1FnJ-NHGqqCgAC6XoNJJCSeOjfUU-TeTc'
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
    - name: Enver Menadjiev
      affiliations:
        name: KAIST
    - name: Youngju Joung
      affiliations:
        name: KAIST
    
bibliography: all.bib
giscus_comments: true
disqus_comments: false
---

## Introduction

Inspection of a complex deep neural network requires a special method. Numerous neurons in the model output signals and identification of the role of the neurons can be done with correlation analysis, that is, if a neuron exhibits positive signals with images of a specific concept, we can say at least the neuron has the conceptual feature. Some might say that the relation is a spurious correlation between the neuron and the concept, but at least we can say there is a positive correlation. An automatic way to conduct this correlation analysis is probing, which learns a simple classification or regression model that inputs an activation of a neuron and outputs a concept. If the probing classifier has a good `test accuracy`. Then, we can say that some neurons connected to the classifier have positive correlation. 
To train probing classifiers, we need specifically designed training dataset and testing dataset. This post describes components required to train probing classifiers; dataset construction,  experiments, and results of probing.  


## Datasets

In the task of image classification, labels of images are provided, and we say concepts for the unlabeled features in images. For example, apple images might have a red color concept, and zebra images might have a striped pattern concept. This intuition requires the identification of concepts in images and these methods are mostly done manually by annotators. However, obtaining such images for this simple work is improper and we use an automatic way of obtaining concepts. We use the CLIP model, which computes image-text similarity. CLIP model is a zero-shot classifier that does not require additional training for matching image-concept. We obtain concept probabilities by computing the normalized cosine similarities between an image and concept labels. In this work, we use three concept groups: 

*  <code style='background-color:#FFFFDD'>Color</code> : [red, yellow, blue, green, none]
*  <code style='background-color:#FFDDFF'>Pattern</code>: [zigzagged, striped, dotted, none] 
*  <code style='background-color:#DDFFFF'>Sex</code> : [male, female, none]

To find a proper concept, we use the label of the form `“This image has a concept: <concept>”`  instead of a single word `<concept>`. 

We compute all the normalized probabilities for 50,000 images in the ImageNet1K validation set. We sort the images with the likelihoods for concepts respectively and select 100 upper images for the positive samples. The negative samples are random 200 images in the excluded set of positive samples. Although it is unclear how to sample, and how much to sample negative samples, we believe random 200 samples are enough to present the probing classifiers. 

Here are 20 images from the first to 100th image with step size 5. 

<table>
</tr>
<tr>
    <td> <img src="https://drive.google.com/uc?export=view&id=12YKj_2j5EAA11u13bl5lpzJEyAJdYRXS" style="width:100%" onClick="window.open(this.src)" role="button"> </td>
    <td> <img src="https://drive.google.com/uc?export=view&id=1FnJ-NHGqqCgAC6XoNJJCSeOjfUU-TeTc" style="width:100%" onClick="window.open(this.src)" role="button"> </td> 
    <td> <img src="https://drive.google.com/uc?export=view&id=1Lq5L4l5gqU7R2khDAK1m9uDcqrN6LxA5" style="width:100%" onClick="window.open(this.src)" role="button"> </td>
    <td> <img src="https://drive.google.com/uc?export=view&id=1GNOXnGONa67d6IxdIfqJY_KPWuJInEW-" style="width:100%" onClick="window.open(this.src)" role="button"> </td>
</tr>
<tr style='text-align:center'>
    <td> <code style='background-color:#FFFFDD'>Red</code> Concept </td>
    <td> <code style='background-color:#FFFFDD'>Blue</code> Concept </td>
    <td> <code style='background-color:#FFFFDD'>Green</code> Concept </td>
    <td> <code style='background-color:#FFFFDD'>Yellow</code> Concept </td>
</tr>
<tr>
    <td> <img src="https://drive.google.com/uc?export=view&id=1kcuUlNltgQfFDB7rrwSYC0goecVYOmIv" style="width:100%" onClick="window.open(this.src)" role="button"> </td>
    <td> <img src="https://drive.google.com/uc?export=view&id=13BhwpGgdMF8G5BAyEfZi9YMrFtH-n5NT" style="width:100%" onClick="window.open(this.src)" role="button"> </td>
    <td> <img src="https://drive.google.com/uc?export=view&id=1DFDe-Q6oa8vbAC9qMeaCfFR8eP6idfll" style="width:100%" onClick="window.open(this.src)" role="button"> </td>
    <td> </td>
</tr>
<tr style='text-align:center'>
    <td> <code style='background-color:#FFDDFF'>Dotted</code> Concept </td>
    <td> <code style='background-color:#FFDDFF'>Striped</code> Concept </td>
    <td> <code style='background-color:#FFDDFF'>Zigzagged</code> Concept </td>
    <td> </td>
</tr>
<tr>
    <td> <img src="https://drive.google.com/uc?export=view&id=1SvldJZq_woAK3KKdP9aRtkWXCP357Oya" style="width:100%" onClick="window.open(this.src)" role="button"> </td>
    <td> <img src="https://drive.google.com/uc?export=view&id=1n95wXWhFRTlzpdxcbIocAw65ssYAzm5J" style="width:100%" onClick="window.open(this.src)" role="button"> </td>
    <td> </td>
    <td> </td>
</tr>
<tr style='text-align:center'>
    <td> <code style='background-color:#DDFFFF'>Female</code> Concept </td>
    <td> <code style='background-color:#DDFFFF'>Male</code> Concept </td>
    <td> </td>
    <td> </td>
</tr>
</table>


## Models 


### Trained Model 

Probing classifiers are for the identification of neurons which are already trained. For the trained neurons, we use a reproduced MobileNetV2 trained on IamgeNet1K with 1~300 checkpoints. The training script is followed by the recipe in torchvision. We used a single A100 GPU and the progress can be found in the W&B report. 

<iframe src="https://api.wandb.ai/links/bumjin/9r7xwq8y"  style="border:1px solid #000000;height:512px;width:100%"/>


### Probing Classifier 

Probing classifiers can be categorized by linearity. A linear probing  classifier directly learns to map activation of neurons to concepts, while a non-linear probing classifier learns the non-linear interaction between activations to verify the relationship between neurons. Following the work <d-cite key="li2022emergent"/> we use ReLU architecture for nonlinear probing classifiers.  

Additional consideration is the choice of neurons in CNN models. We can directly map the receptive field of CNN outputs or reduce them to make a single scalar in a channel. We separate these two cases as “gap” and “non-gap” which indicates the global average pooled representation or not. 

## Results

All the results provided in this section are test accuracy of $30\%$ of the positive and negative samples. 
We report two types of results: what-where-when plot which shows the progress of concept emergence over training epochs and modules for each concept and test accuracies of `"linear-gap"`, `"non_linear-gap"` and `"non_linear-non_gap"`

### What-Where-When Plot

*  <code style='background-color:#FFFFDD'>Color</code> : [red,  blue, green]
*  <code style='background-color:#FFDDFF'>Pattern</code>: [zigzagged, striped, dotted] 
*  <code style='background-color:#DDFFFF'>Sex</code> : [male, female]

#### CAVEAT 

We observe that the when (training epoch) has no difference for the test accuracy. We double checked the code, but the results are same. Therefore, we could not clearly say about the `when` part now. 


<div style="width:900px;">
    <div style="float:left; width:450px;" >
        
        <h3> 🏖️ Linear-GAP </h3>
        <iframe  src="{{ '/assets/plotly/linear_gap_concepts.html' | relative_url }}" frameborder='0' scrolling='yes' height="800px" width="100%" style="border:1px solid #EE00DD55;"></iframe>
    </div>
  <div style="float:right; width:450px;">
    <h3> 🏖️ Non-Linear-None-GAP </h3>
    <iframe  src="{{ '/assets/plotly/non_linear_non_gap_concepts.html' | relative_url }}" frameborder='0' scrolling='yes' height="800px" width="100%" style="border:1px solid #EE00DD55;"></iframe>
    </div>
  <div style="clear:both;"></div>
</div>


## Tables (Test Acc)

### Color Concepts

<div class="table-slider" markdown="1" style="width:60rem">
<div markdown="1">
<h3> Red </h3>

|layer| linear_gap | non_linear_gap | non_linear_non_gap |
|---|---|---|---|
module:0|0.862|0.919|**0.981**|
module:1|**0.990**|**0.990**|0.986|
module:2|0.910|0.929|**0.981**|
module:3|0.910|0.929|**0.981**|
module:4|0.924|0.919|**0.981**|
module:5|0.919|0.919|**0.971**|
module:6|0.929|0.929|**0.962**|
module:7|0.933|0.952|**0.981**|
module:8|0.924|**0.971**|**0.971**|
module:9|0.929|**0.976**|0.962|
module:10|0.929|**0.976**|0.962|
module:11|0.938|**0.981**|0.952|
module:12|0.938|**0.976**|0.967|
module:13|0.948|**0.976**|0.967|
module:14|0.971|**0.986**|0.971|
module:15|0.981|**0.990**|0.976|
module:16|0.981|**0.986**|0.971|
module:17|**0.990**|**0.990**|0.986|
module:18|**0.990**|**0.990**|0.981|

</div>

<div markdown="1">
<h3> Blue </h3>

|layer| linear_gap | non_linear_gap | non_linear_non_gap |
|---|---|---|---|
module:0|0.686|0.829|**0.971**|
module:1|0.962|**0.981**|0.971|
module:2|0.748|0.838|**0.957**|
module:3|0.743|0.876|**0.957**|
module:4|0.790|0.871|**0.952**|
module:5|0.829|0.871|**0.952**|
module:6|0.824|0.886|**0.952**|
module:7|0.852|0.895|**0.957**|
module:8|0.848|0.919|**0.962**|
module:9|0.824|0.919|**0.957**|
module:10|0.810|0.938|**0.967**|
module:11|0.843|**0.957**|0.952|
module:12|0.852|0.938|**0.957**|
module:13|0.876|0.948|**0.962**|
module:14|0.876|**0.976**|0.971|
module:15|0.848|**0.967**|0.962|
module:16|0.881|**0.971**|0.962|
module:17|0.962|**0.981**|0.971|
module:18|0.962|**0.967**|0.957|

</div>

<div markdown="1">
<h3> Green </h3>

|layer| linear_gap | non_linear_gap | non_linear_non_gap |
|---|---|---|---|
module:0|0.671|0.881|**0.962**|
module:1|0.962|**0.981**|**0.981**|
module:2|0.852|0.910|**0.938**|
module:3|0.862|0.895|**0.952**|
module:4|0.876|0.924|**0.967**|
module:5|0.886|0.938|**0.962**|
module:6|0.890|0.948|**0.967**|
module:7|0.919|0.943|**0.981**|
module:8|0.919|0.933|**0.948**|
module:9|0.895|0.948|**0.952**|
module:10|0.881|**0.962**|0.952|
module:11|0.900|0.957|**0.976**|
module:12|0.914|0.943|**0.967**|
module:13|0.910|**0.962**|0.933|
module:14|0.929|0.967|**0.981**|
module:15|0.905|**0.962**|0.948|
module:16|0.914|**0.967**|**0.967**|
module:17|0.962|**0.967**|**0.967**|
module:18|0.962|**0.981**|0.943|

</div>

<div markdown="1">
<h3> Yellow </h3>

|layer| linear_gap | non_linear_gap | non_linear_non_gap |
|---|---|---|---|
module:0|0.757|0.819|**0.948**|
module:1|0.976|**0.981**|0.962|
module:2|0.800|0.829|**0.933**|
module:3|0.805|0.810|**0.933**|
module:4|0.819|0.819|**0.924**|
module:5|0.814|0.843|**0.933**|
module:6|0.833|0.838|**0.924**|
module:7|0.824|0.833|**0.943**|
module:8|0.829|0.852|**0.933**|
module:9|0.843|0.886|**0.933**|
module:10|0.857|0.886|**0.943**|
module:11|0.814|**0.919**|**0.919**|
module:12|0.857|0.905|**0.924**|
module:13|0.848|0.905|**0.910**|
module:14|0.919|**0.981**|0.952|
module:15|0.933|**0.962**|0.952|
module:16|0.933|**0.976**|0.933|
module:17|**0.971**|0.957|0.962|
module:18|0.976|**0.981**|0.962|

</div>
</div>

<hr>

### Pattern Concepts

<div class="table-slider" markdown="1" style="width:60rem">

<div markdown="1">
<h3> Striped </h3>

|layer| linear_gap | non_linear_gap | non_linear_non_gap |
|---|---|---|---|
module:0|0.667|0.671|**0.890**|
module:1|0.995|**1.000**|0.995|
module:2|0.800|0.800|**0.933**|
module:3|0.805|0.810|**0.924**|
module:4|0.838|0.871|**0.952**|
module:5|0.838|0.862|**0.948**|
module:6|0.843|0.886|**0.924**|
module:7|0.890|0.943|**0.957**|
module:8|0.900|0.914|**0.971**|
module:9|0.914|0.938|**0.962**|
module:10|0.910|**0.957**|**0.957**|
module:11|0.905|0.952|**0.967**|
module:12|0.905|0.967|**0.976**|
module:13|0.938|**0.976**|0.948|
module:14|0.957|0.986|**0.990**|
module:15|0.976|**0.990**|0.986|
module:16|0.995|**1.000**|0.990|
module:17|**0.995**|0.990|0.990|
module:18|0.990|**1.000**|0.995|

</div>

<div markdown="1">
<h3> Zigzagged </h3>

|layer| linear_gap | non_linear_gap | non_linear_non_gap |
|---|---|---|---|
module:0|0.667|0.676|**0.900**|
module:1|0.981|**0.986**|0.976|
module:2|0.714|0.719|**0.914**|
module:3|0.705|0.743|**0.895**|
module:4|0.786|0.795|**0.929**|
module:5|0.800|0.795|**0.905**|
module:6|0.795|0.800|**0.919**|
module:7|0.814|0.857|**0.924**|
module:8|0.814|0.886|**0.933**|
module:9|0.843|0.886|**0.938**|
module:10|0.833|0.905|**0.943**|
module:11|0.871|0.933|**0.943**|
module:12|0.886|0.938|**0.943**|
module:13|0.890|0.938|**0.957**|
module:14|0.924|**0.952**|0.948|
module:15|0.914|0.952|**0.957**|
module:16|0.938|0.957|**0.976**|
module:17|0.971|**0.986**|0.976|
module:18|**0.981**|0.976|0.957|

</div>

<hr>
<div markdown="1">
<h3> Dotted </h3>

|layer| linear_gap | non_linear_gap | non_linear_non_gap |
|---|---|---|---|
module:0|0.667|0.695|**0.905**|
module:1|0.976|**0.995**|**0.995**|
module:2|0.719|0.767|**0.933**|
module:3|0.724|0.748|**0.952**|
module:4|0.757|0.867|**0.976**|
module:5|0.781|0.867|**0.981**|
module:6|0.790|0.881|**0.976**|
module:7|0.838|0.929|**0.971**|
module:8|0.852|0.924|**0.995**|
module:9|0.795|0.929|**0.990**|
module:10|0.752|0.952|**0.976**|
module:11|0.767|0.952|**0.995**|
module:12|0.805|0.962|**0.990**|
module:13|0.848|0.967|**0.995**|
module:14|0.833|0.986|**0.995**|
module:15|0.905|0.986|**0.995**|
module:16|0.924|0.986|**0.995**|
module:17|0.933|0.990|**0.995**|
module:18|0.976|**0.995**|**0.995**|

</div>
</div>

<hr>

### Gender Concepts

<div class="table-slider" markdown="1" style="width:60rem">
<div markdown="1">
<h3> Male </h3>

|layer| linear_gap | non_linear_gap | non_linear_non_gap |
|---|---|---|---|
module:0|0.681|0.733|**0.943**|
module:1|0.976|**0.981**|0.971|
module:2|0.781|0.824|**0.924**|
module:3|0.810|0.824|**0.943**|
module:4|0.829|0.838|**0.929**|
module:5|0.871|0.824|**0.933**|
module:6|0.876|0.838|**0.943**|
module:7|0.871|0.881|**0.938**|
module:8|0.876|0.900|**0.943**|
module:9|0.867|0.914|**0.943**|
module:10|0.881|0.952|**0.957**|
module:11|0.886|0.933|**0.943**|
module:12|0.890|**0.943**|**0.943**|
module:13|0.895|0.938|**0.952**|
module:14|0.924|**0.962**|0.952|
module:15|0.938|0.952|**0.967**|
module:16|0.948|**0.971**|0.962|
module:17|0.976|**0.981**|0.971|
module:18|**0.971**|0.967|0.967|

</div>

<div markdown="1">
<h3> female </h3>

|layer| linear_gap | non_linear_gap | non_linear_non_gap |
|---|---|---|---|
module:0|0.667|0.686|**0.848**|
module:1|0.938|0.938|**0.952**|
module:2|0.724|0.729|**0.881**|
module:3|0.752|0.729|**0.914**|
module:4|0.724|0.752|**0.881**|
module:5|0.729|0.757|**0.886**|
module:6|0.729|0.767|**0.886**|
module:7|0.757|0.810|**0.905**|
module:8|0.771|0.852|**0.895**|
module:9|0.776|0.852|**0.895**|
module:10|0.776|0.848|**0.890**|
module:11|0.800|0.843|**0.905**|
module:12|0.814|0.843|**0.886**|
module:13|0.833|0.910|**0.914**|
module:14|0.843|**0.933**|0.924|
module:15|0.852|**0.938**|0.910|
module:16|0.895|**0.929**|**0.929**|
module:17|0.938|0.938|**0.952**|
module:18|0.929|0.929|**0.943**|

</div>
</div>


## Conclusion 

This work shows how to construct probing dataset from CLIP and how to evaluate several probing models over all layers. We believe that our work can help readers to understand the overall pipeline of probing classifiers. 