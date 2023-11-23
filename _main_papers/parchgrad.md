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

**ParchGrad: Controlled Internal Gradients for Reliable Saliency Map 🚀**
<div class="authors">Bumjin Park, Giyeong Jeon, and Jaesik Choi, 2023  
</div>
<h3 class="demo-title"> TDLR; </h3>  
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
    <a class="box-demo-link" href="https://drive.google.com/file/d/1i4o9I2DwrN-XEJr0_WHmVEmX4i_3N3rT/view?usp=drive_link" style="background:#AA00AA" >Pre-print (currently google drive)</a>  
      <a class="box-demo-link" href="https://github.com/fxnnxc/Parchgrad" style="background:#000000;">Code</a> 
    <a class="tooltip-wrap">
      <span class="tooltip-span"> Abstract </span>
      <div class="tooltip-content" style="line-height:1.0rem"> 
      <strong> Abstract </strong> <br>
      Explaining the decision of neural networks is crucial to ensure their reliability in various image tasks. Among several input attribution methods, the saliency map explains the decision with the propagated gradients on pixels. One of the fundamental problems of the saliency map is gradient shattering which leaves noise on pixels and hinders the interpretation of attribution maps. To provide a clear explanation, controlling the internal gradient signals is essential. However, to the best of our knowledge, there is no work on modifying the internal gradient signals so that the saliency map provides reliable explanations. To initialize the control of internal gradient signals in the saliency map, we provide theoretical analysis on the controllable modification of gradients in multiple convolutional channels and propose ParchGrad, which prunes channels by partitioning multiple channels into two groups. In addition, we propose a class-wise selection method based on statistical tests to select which channels to magnify or reduce the impact of gradients.
      </div>
    </a> 
  </div> 
    </center>

## Visualization of ParchGrad vs Gradient 

<div style="border:1px solid #DDDDFF;border-radius:10px;padding:10px;" >
<center>
<img src="https://drive.google.com/uc?export=view&id=1XqsOv2ZrZV0H3dnySdKTI1Pz8mk__fuQ" style="width:100%">
<img src="https://drive.google.com/uc?export=view&id=1OFHpMBm16EmzpfJMHgvITQIoXwdr9vL0" style="width:100%">
<img src="https://drive.google.com/uc?export=view&id=1Ez2Vra_1PFtw1uynMuIeaF9Skz2Agb9C" style="width:100%">
<img src="https://drive.google.com/uc?export=view&id=12zZ48LavQK3HCKOTEERTGf8vmrvTSuap" style="width:100%">
<img src="https://drive.google.com/uc?export=view&id=1bSZS_DjN9qXNW2DooNPxwkPdwbRhCg6z" style="width:100%">
<img src="https://drive.google.com/uc?export=view&id=1bdkQmtOvIOtXQbDtACKQcBiOWron-lw5" style="width:100%">
</center>
</div>



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

To theoretically understand the pruning and impact of gradients, we define $\gamma$-dominance and provides theorems to preserve the magnitude of the gradients. Please see the paper for the detailed definition and the theorems. 


## Conclusion

Gradient signal is crucial in explainable AI for various reasons including training and explaining. As the saliency map is model-agnostic, gradient signals are the first option to explain models. However, as gradients are noisy, users cannot believe easily whether the attribution is reliable. To provide a more reliable saliency map, tackling the internal mechanism of gradients and controlling the signals is essential. This paper provides a theoretical and empirical analysis of the gradients so that the XAI community can clarify how the gradients impact the saliency map and how to control them. In detail, this paper shows the impact of variance over many channels and provides a solution to safely turn off the channel impact while conserving the magnitude with other modules.  

We also propose class-wise channel selection, which selects channels based on the statistical importance of GAP. This selection could be understood as adding a forward bias in the backward (adding activation information to gradient computation). We acknowledge that class-wise selection bias is not the best option, but possibly a good option when there is no knowledge of channels. For the ongoing research, we will study other channel selection methods, such as CLIP-based or circuit-based channel selection, so that constructing proper channel propagation paths improves the explainability of the saliency maps. 

We believe ParchGrad can broaden the overall understanding of saliency maps in many image application fields where obtaining a reliable saliency map is important. 