---
layout: distill
title:   ' ParchGrad: Controlled Internal Gradients for Reliable Saliency Map 🚀' 
date: 2023-11-15
giscus_comments : true
description: 
bibliography: all.bib
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST


img: https://drive.google.com/uc?export=view&id=1kJwgA-XPdP0k3cd60R8H7jdqtgLbLJ44
---



<h3 class="demo-title"> TDLR; </h3>
  <div class="authors">Bumjin Park, Giyeong Jeon, and Jaesik Choi, 2023  
  </div>
  <div style="display: grid;grid-template-columns: 1fr 1fr;border:1px solid #DDDDFF;border-radius:10px;padding:10px;align-items:center;">
    <div>
    <ul>
    <li> <strong> Motivation </strong> : Gradients are shattered, and more refined internal signals can improve the saliency map. </li> 
    <li> <strong> Problem </strong> : Comparison of internal gradients in a convolutional layer and make a measure properly compare gradient signals.  </li>
    <li> <strong> Method </strong> : We propose $\gamma$-dominance based on the random variable formulation of gradients. </li>
    <li> <strong> Contribution </strong> : We propose variance conservation and class-wise channel selection to safely prune internal gradient signals. </li>
    <li> <strong> Keywords </strong> : saliency map, channel pruning </li>
    </ul>
      </div>
  </div>

  <center>
  <div class="demolink" style='padding-bottom:1rem;padding-top:0.5rem;'>
    <a class="box-demo-link" href="https://drive.google.com/file/d/1i4o9I2DwrN-XEJr0_WHmVEmX4i_3N3rT/view?usp=drive_link" style="background:#AA00AA" >Pre-print (currently google drive)</a>  <br>
      <a class="box-demo-link" href="https://github.com/fxnnxc/Parchgrad" style="background:#000000;">Code</a> 
    <a class="tooltip-wrap"><br>
      <span class="tooltip-span"> Abstract </span>
      <div class="tooltip-content"> 
      <strong> Abstract </strong> <br>
      Explaining the decision of neural networks is crucial to ensure their reliability in various image tasks. Among several input attribution methods, the saliency map explains the decision with the propagated gradients on pixels. One of the fundamental problems of the saliency map is gradient shattering which leaves noise on pixels and hinders the interpretation of attribution maps. To provide a clear explanation, controlling the internal gradient signals is essential. However, to the best of our knowledge, there is no work on modifying the internal gradient signals so that the saliency map provides reliable explanations. To initialize the control of internal gradient signals in the saliency map, we provide theoretical analysis on the controllable modification of gradients in multiple convolutional channels and propose ParchGrad, which prunes channels by partitioning multiple channels into two groups. In addition, we propose a class-wise selection method based on statistical tests to select which channels to magnify or reduce the impact of gradients.
      </div>
    </a> 
  </div> 
    </center>

## Introduction 

Explaining the decision of neural networks is crucial to ensure their reliability in various
image tasks. Among several input attribution methods, the saliency map explains the
decision with the **propagated gradients on pixels**. One of the fundamental problems
of the saliency map is gradient shattering which **leaves noise on pixels and hinders the interpretation of attribution maps**. 

To provide a clear explanation, controlling the internal gradient signals is essential. However, to the best of our knowledge, there is
no work on **modifying the internal gradient signals** so that the saliency map provides
reliable explanations. 

To initialize the control of internal gradient signals in the saliency
map, we provide theoretical analysis on the controllable modification of gradients in
multiple convolutional channels and propose ParchGrad, which prunes channels by
partitioning multiple channels into two groups. In addition, we propose a class-wise
selection method based on statistical tests to select which channels to magnify or
reduce the impact of gradients.

<center>
<img src="https://drive.google.com/uc?export=view&id=1kJwgA-XPdP0k3cd60R8H7jdqtgLbLJ44" style="width:70%">
</center>


### Contribution: Comparison with other explainable methods.

|Method  | Category  | All layers | Prune Channels | #Propagation* | Sample Size |
| --- | --- | :---:| :---: | :---: | :---: | 
ScoreCAM <d-cite key="wang2020score" >  </d-cite> | CAM  | X | O | O(C) | O(C)
AblationCAM <d-cite key="ramaswamy2020ablation" >  </d-cite>  | CAM | X | O | O(C) | O(C)
LRP  <d-cite key="bach2015pixel" >  </d-cite> | LRP | O | X | O(1) | O(1)
CRP <d-cite key="achtibat2022towards" >  </d-cite>  |  LRP | O | O | O(1) | O(1)
Gradient <d-cite key="adebayo2018sanity" >  </d-cite>  | Saliency | O | X | O(1) | O(1)
InputGrad <d-cite key="adebayo2018sanity" >  </d-cite>  | Saliency | O | X | O(1) | O(1)
Integrated Gradient <d-cite key="sundararajan2017axiomatic" >  </d-cite>  | Saliency | O | X | O(1) | O(M)
SmoothGrad <d-cite key="smilkov2017smoothgrad" >  </d-cite>  | Saliency | O | X | O(M) | O(M)
**ParchGrad (proposed)** | Saliency | O | O | O(1) | O(1)

 * **"All Layers"** refers to whether including most of convolutional layers in a model for the explanation, 
 * **"Pruning"** refers to whether manual modification is applied, 
 * **"Propagation"** is the complexity of individual back-propagation to obtain the explanation. 
 * **"Sample-size"** is the total number of samples to obtain the explanation. 


## Problems 

### 1. Gradients Are Noisy

Gradients are noisy to be interpreted as large number of small signals could exists on a input. A toy experiment below shows the impact of noisy gradients, which are randomly distributed.  

<center>
<img src="https://drive.google.com/uc?export=view&id=14RK0ctQBWL5c9VPVxmxKlF4DXZzVCdPV" style="width:40%">
</center>

Let $\mathcal{H}$ be a group of 10  random gradients whose first entry is sampled from $\mathcal{N}(0, 0.1)$ and the second entry is sampled from $\mathcal{N}(0.0, 1.0)$ and $\mathcal{L}$ be a group of 100 random gradients  whose elements are both sampled from  N(0, 0.1). The summed vector in each group and the combined group is shown in Figure below. Although H group is dominant in Dim2 (0,1), the summed vector of L group is dominant in Dim1 (1,0)  because of large number of vectors. Consequently, the final direction is (1,1) direction (black). This experiment shows the impact of large number of irrelevant channels (red).



### 2. Zero Pruning of Gradients Result in Artifacts 

One trivial method to remove noisy gradients is to set zero. However, we observe that the zero gradient signals on selected channels results in artifacts. 

<center>
<img src="https://drive.google.com/uc?export=view&id=14o_c9CdgiRQTrdKcgwoFOltODIvDoRPT" style="width:60%">
</center>

The artifacts are due to the fact that multiple multiple modules are interact to make gradient signal. When zero pruning is applied to the gradients in a convolutional layer, the impact of the convolutional layer is **reduced** and lost the dominance over other modules. 

<center>
<img src="https://drive.google.com/uc?export=view&id=1YF7QIJh1S8LjvxrFt1lCKQIEPkpEw0xl" style="width:80%">
</center>

## Methods : $\gamma$-dominance and variance conservation

To theoretically understand the pruning and impact of gradients, we define $\gamma$-dominance and provides theorems to preserve the magnitude of the gradients. 
Please see the paper for the detailed definition and the theorems. 

## Experiments 


### Qualitative 

<img src="https://drive.google.com/uc?export=view&id=1p9-NL-buI_lqy7nTNg7Jvhc2z_L8Y-Si" style="width:100%">


### Quantitative

<img src="https://drive.google.com/uc?export=view&id=1ZX9REnQ7Cw4Mnqzk2Q8afkedXDrX9o6U" style="width:100%">


## Conclusion
