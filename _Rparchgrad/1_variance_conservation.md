---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2023-08-24
featured: true
img: https://drive.google.com/uc?export=view&id=14RK0ctQBWL5c9VPVxmxKlF4DXZzVCdPV
title: '[1] Motivation of The Problem.'
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


📌 Code Release : [parchgrad/v23.11.07.1](https://github.com/fxnnxc/parchgrad/tree/v23.11.07.1)


## Gradient Aggregation Phenomena 

[2023.11.05]  **The impact of large number of small magnitude gradients.** 

As the sign of gradients are changed by the normally distributed weights, the impact of multiple gradients are be magnified by the variance computation in the summation of multiple random variables. As a result, large number of small signals has high impact. The Figure below shows an example of the impact. We sample 10 high magnitude gradients $\mathcal{H}$ (blue) and 100 gradients $\mathcal{L}$ of 10 times smaller magnitude. Unlike our intuition that the dominance of $\mathcal{H}$ should be obtained, the resulted direction is highly affected by $\mathcal{L}$ (see Appendix for details). 


<center>
<img src="https://drive.google.com/uc?export=view&id=14RK0ctQBWL5c9VPVxmxKlF4DXZzVCdPV" style="width:60%">
<figcaption> Although random gradients from $\mathcal{L}$ group has small magnitude, the large number of gradients results in high magnitude that change the direction from the blue arrow to the black arrow. 
</figcaption>
</center>

Let $\mathcal{H}$ be a group of 10 random gradients whose first entry is sampled from $\mathcal{N}(0, 0.1)$ and the second entry is sampled from $\mathcal{N}(0.0, 1.0)$ and $\mathcal{L}$ be a group of 100 random gradients  whose elements are both sampled from  $\mathcal{N}(0, 0.1)$. The summed vector in each group and the combined group is shown in Figure below. Although \mathcal{H} group is dominant in Dim2 $(0,1)$, the summed vector of $\mathcal{L}$ group is dominant in Dim1 $(1,0)$  because of large number of vectors. Consequently, the final direction is $(1,1)$ direction (black). This experiment shows the impact of large number of irrelevant channels (red).


## Conserve Magnitude

<center>
<img src="https://drive.google.com/uc?export=view&id=1hmYELKr6i3ZFl7MtCZW_E6vc30cTCaWd" style="width:100%">
</center>


#### ResNet18 

For the ResNet18, conservation of variance results in gradient signals on more bounding box regions. As the small number of channels are used in the convolutional modules, more gap is more obtained.  

<center>
<img src="https://drive.google.com/uc?export=view&id=1ExjOnUQfTXtF2lzc6uO1iY7XeSY1-iuZ" style="width:80%">
</center>

#### VGG16 

As VGG16 has only sequential gradient computation the conservation of gradient variance has no effect. 

<center>
<img src="https://drive.google.com/uc?export=view&id=1ThpX5NAEhbUEEByTTSwL84Xc2YMZA2Oh" style="width:80%">
</center>

#### EfficientNetB0

ResNet has more submodules with different kernel size. We conjecture that filtering convolutions in EfficientNet requires more discussions. 

<center>
<img src="https://drive.google.com/uc?export=view&id=1KJMlSNbBJJNz2D3YFF5Uza4eRW5nr2-F" style="width:80%">
</center>






### Experimental Setting 

The above experiment is done with filtering sequential convolutions. However, in the paper, we filter only upper parts to remove unrelated concepts. 

<div style='border:1px solid #DDDDDD;padding:10px; border-radius:10px;' >
<span> Hook Modules </span>
<d-code language='python'>
# ResNet18
selected_convolutions = []
for layer in [self.model.layer3, self.model.layer4]:
    for basic_block in layer:
        for name in ['conv1', 'conv2', 'conv3']:
            if hasattr(basic_block, name):
                conv = getattr(basic_block, name)
                selected_convolutions.append(conv)

# VGG16
selected_convolutions = []
num = len(self.model.features)
start_num = int(num*0.5)
print("[INFO] hook upper half convolutions")
for n in range(start_num, num):
    layer=self.model.features[n]
    if layer.__class__.__name__ == "Conv2d":
        selected_convolutions.append(layer)

# EfficientNet B0
selected_convolutions = [] 
for num in [6,7]:
    layer = self.model.features[num]
    conv = layer[0].block[1][0]
    selected_convolutions.append(conv)
selected_convolutions.append(self.model.features[8][0])
</d-code >
</div>