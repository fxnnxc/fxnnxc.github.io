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
img: /assets/experiments/gpt_sip_twitter/70m_train_ppl.png
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


[2023.11.05]  The impact of large number of small signal channels. 

Let H be a group of 10  random gradients whose first entry is sampled from N(0, 0.1) and the second entry is sampled from N(0.0, 1.0) and L be a group of 100 random gradients  whose elements are both sampled from  N(0, 0.1). The summed vector in each group and the combined group is shown in Figure below. Although H group is dominant in Dim2 (0,1), the summed vector of L group is dominant in Dim1 (1,0)  because of large number of vectors. Consequently, the final direction is (1,1) direction (black). This experiment shows the impact of large number of irrelevant channels (red).



<img src="https://drive.google.com/uc?export=view&id=14RK0ctQBWL5c9VPVxmxKlF4DXZzVCdPV" style="width:70%">

<img src="https://drive.google.com/uc?export=view&id=1KJMlSNbBJJNz2D3YFF5Uza4eRW5nr2-F" style="width:70%">

<img src="https://drive.google.com/uc?export=view&id=1KJMlSNbBJJNz2D3YFF5Uza4eRW5nr2-F" style="width:70%">

<img src="https://drive.google.com/uc?export=view&id=1ThpX5NAEhbUEEByTTSwL84Xc2YMZA2Oh" style="width:70%">

<img src="https://drive.google.com/uc?export=view&id=1hmYELKr6i3ZFl7MtCZW_E6vc30cTCaWd" style="width:70%">




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