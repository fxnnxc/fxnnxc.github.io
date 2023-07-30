---
layout: share-distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
    - name : Enver Menadjiev
      affiliations:
        name: KAIST
    - name : Youngju Joung
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-07-25
featured: true
# toc:
#   - name: Problems
title: 'Curriculum of Interpretability Team'
description: 'Contents to follow the learning of interpretability. '
img:
---

# Curriculum 


| Chapter |  Section |
|:------:|-----------|---------------|
| **Chaper 1. Neuron** | Neurons in Neuroscience |
|     |  Neurons in Deep Learning |
|     |  Interpret Neurons in MLP |
|     |  Interpret Neurons in CNN |
|     |  Interpret Neurons in RNN |
|     |  Interpret Neurons in Transformer |
|     | Probing Neurons | 
|     | Class Activation Vector |  
|||
|  ???   |  Logit Lens |
| **CNN** | Channels | 
| **CNN** | Feature Detector | 
| **CNN** | Circuits | 
|||
| **Transformer**       |   Tokens      | 
| **Transformer**       |   Blocks      | 
| **Transformer**       |   Attention         | 
| **Transformer**       |   MLP               | 
| **Transformer**       |   Key-Value Memory  | 




# Tools 

## 🪴 Neuron Interpretation

<div style="display:block; grid-column:middle; width:120%;margin-left:-40px;padding-right:50px;" markdown="1" >

* **Linear Probing** : determine whether a neuron has concept with a linear classifier <d-cite key="mcgrath2022acquisition"/> <d-cite key="li2022emergent"/>.
* **Non-Linear Probing** : probing with a non-linear classifier <d-cite key="li2022emergent"/>. 
* **CAV** (Concept Activation Vector) : probing with concept labels <d-cite key="mcgrath2022acquisition"/>.
* **Logit Lens**: directly maps neuron to the logits <d-cite key="leike2023language"/>.
* **Feature Visualization** (Deep Dream) gradient ascent input to maximize the neuron <d-cite key="mordvintsev2015inceptionism"/><d-cite key="olah2018the"/>.
* **Activation Atlas**: applying U-Map on the activation vectors and put feature visualization on each points  <d-cite key="carter2019activation"/><d-cite key="olah2018the"/>.
* **What-When-Where visualization**: CAV test accuracy  for all layers, training epoch and concept <d-cite key="mcgrath2022acquisition"/>.
* **Network Dissect**: find neurons via heatmap overlapping.
* **CCD** (Concept Confidence Deviation) : <d-cite key="patel2023conceptbed"/>.
* **Latent Saliency Maps**: <d-cite key="li2022emergent"/>.
* **Integrated Gradient** : 

</div>

## 🪴  Representation Interpretation

<div style="display:block; grid-column:middle; width:120%;margin-left:-40px;padding-right:50px;" markdown="1" >

* **NMF** (Non-negative Matrix Factorization) : reducing the number of items to the defined numbers <d-cite key="schubert2021high-low"/><d-cite key="olah2018the"/>.
* **Activation Intervention** (Activation Patch) :  <d-cite key="li2022emergent"/><d-cite key="meng2022locating"/> 
* **t-SNE** : <d-cite key="wattenberg2016how"/>
* **U-MAP**

</div>


## 🪴  Architecture Interpretation

<div style="display:block; grid-column:middle; width:120%;margin-left:-40px;padding-right:50px;" markdown="1" >

* **Transformer** 
* **Attention** 
* **Self-Attention**
* **Cross-Attention**
* **CNN** 
* **RNN** 
* **Gated RNN**
* **MLP**
* **Linear Layer**
* **Pooling** 

</div>



# Concepts 

<div style="display:block; grid-column:middle; width:120%;margin-left:-40px;padding-right:50px;" markdown="1" >

| Concept | Summary | Related Papers | 
| --------|--------|--------|
| Grokking | The phenomena that generalization is slower than training performance|  -- |
| Surface Statistics | Model relies on spurious correlations.  <br> A long list of correlations that do not reflect a causal model of the process generating the sequence. |  <d-cite  key="li2022emergent"/>| 


</div>

