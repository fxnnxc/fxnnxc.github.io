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

<div style="display:block; grid-column:middle; width:120%;margin-left:-40px;padding-right:50px;" markdown="1" >
<!-- <div style="display:block;  width:100%" markdown="1" > -->

| Tool | Tag |   Summary  | Related Papers |
|-----:|-----------|---------------|:---| 
| Linear Probing | `` |  |   <d-cite key="mcgrath2022acquisition"/><d-cite key="li2022emergent"/>  | 
| Non-Linear Probing | `` |  |   <d-cite key="li2022emergent"/>  | 
| CAV (Concept Activation Vector) |  - - | - |  <d-cite key="mcgrath2022acquisition"/> |
| Logit Lens | - | - | 
| NMF (Non-negative Matrix Factorization) | `Dimension Reduction`  | - | 
| Feature Visualization (Deep Dream) |  `Neuron` |
| Activation Atlas |   ? | Summary | - |
| Decision Boundary |  `Neuron`  | 
| What-When-Where visualization  | `Neuron` | CAV Test accuracy  for all layers, training epoch and concept    | <d-cite key="mcgrath2022acquisition"/> | 
| Network Dissect | `Neuron`  | Find neurons via heatmap overlapping | |
| CCD (Concept Confidence Deviation) |`Metric` |  | <d-cite key="patel2023conceptbed"/>| 
| Integrated Gradient | `Attribution`| Summary | - | 
| Latent Saliency Maps | `Attribution` |  Summary  | <d-cite key="li2022emergent"/>  | 
| Activation Intervention <br> Activation Patch | `Neuron` | Summary | <d-cite key="li2022emergent"/><d-cite key="meng2022locating"/>  | 



</div>


# Concepts 

<div style="display:block; grid-column:middle; width:120%;margin-left:-40px;padding-right:50px;" markdown="1" >

| Concept | Summary | Related Papers | 
| --------|--------|--------|
| Grokking | The phenomena that generalization is slower than training performance|  -- |
| Surface Statistics | Model relies on spurious correlations.  <br> A long list of correlations that do not reflect a causal model of the process generating the sequence. |  <d-cite key="li2022emergent"/> | 


</div>

