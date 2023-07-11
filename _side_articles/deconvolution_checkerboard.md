---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: deconvolution_checkerboard.bib
giscus_comments: true
disqus_comments: true
date: 2023-07-10
featured: true
img: /assets/side_articles/deconvolution/checkboard_example.png
toc:
  - name: Introduction
    # if a section has subsections, you can add them as follows:
  - name: Convolution and Deconvolution
    subsections:
      - name: Convolution
      - name: Deconvolution
  - name: Resize Convolution 
  - name: Conclusion 
title: 'Deconvolution and Checkboard Artifacts'
description: 'This is an informal review of a paper. We take computational analysis of convolution and deconvolution for artifact generation'
---

> Note: This post is a mathematical review of the paper "Deconvolution and Checkerboard Artifacts" [[project link](https://distill.pub/2016/deconv-checkerboard)] <d-cite key="odena2016deconvolution"/>, Intended to  broaden the understanding of operations, convolution and deconvolution. We use © symbol to represent the texts and figures obtained from the paper. <text style="font-style:normal"> 🤗 </text>

> The motivation of this review is Circuits Thread <d-cite key="cammarata2020thread"/>. As modules in NN propagate signals, they can make artifacts, which may be the results of the inductive bias. Therefore, we  broaden the understanding of mechanistic interpretability by analyzing the modules in a deeper manner. 

## Introduction 

The most widely used form of generative models is "Convolution - Deconvolution" structure. This structure has a bottleneck in the middle so that the compressed representation of an image can be encoded properly. At the stage of convolution layers, the information of an image is extracted. The stage of deconvolution layers recovers the image signals from the bottleneck representation by progressively widening the image sizes (normally double). 
<figure style="text-align:center">
<img src="/assets/side_articles/deconvolution/checkboard_example.png" style="width:30%">
<figcaption>
  Figure 1. Checkerboard artifacts generated from the conv-deconv architecture. Even though conv captures the semantic well, the generative parts (decov) makes artifacts.  ( © Figure from <a style="font-size:13px" href="https://distill.pub/2016/deconv-checkerboard/">the original post </a>)
</figcaption>
</figure>

The impact of deconvolution has been studied by Odena et al. in the paper "Deconvolution and Checkerboard Artifacts" [[project link](https://distill.pub/2016/deconv-checkerboard)]. The major problem is the overlapping signals from deconvolution operation. That is, deconvolution layers produce checkerboard artifacts in the output. Checkerboard artifacts are not representative features of an image because these are high frequency signals repeated locally which are unusual natural representations. 
The interpretable reason of the checkerboard patterns is the deconvolution layer which spreads the input signals with convolutional filters. Note that deconvolution is a reverse operation of a convolutional layer which distributes the output signals to the input regions. In the process of distribution, the imbalance of signal distribution happens. 
To better understand the reasons of checkerboard artifacts, we analyze how convolutional and deconvolutional layers propagate the input signals. In addition, we review the "Resize-Convolution"  which is an alternative operation for deconvolution. 





## Convolution and Deconvolution 

We first analyze how convolution and deconvolution 

* **Convolution** : Gathers the input signals
* **Deconvolution** : :star:Spreads the input signals

See the Figures below for 1D examples. 

<div class="card" style="width:auto;padding:30px;margin-top:20px" markdown="1">
<h1 style='text-align:left'> 🚀 Convolution and Deconvolution    </h1>
#### 📌 Figure: (Left)
* Convolution Operation (`Kernel=3`, `Stride=1`) 
* Pixels of `1~7` are convolved with filter `[1,2,3]`
* (Matrix) : For each pixel position, the accumulated signals are stacked.

#### 📌 Figure: (Right)
* DeConvolution Operation (`Kernel=3`, `Stride=1`) 
* Pixels of `1~5` are deconvolved  with filter `[1,2,3]`
* (Matrix) : For each pixel position, the accumulated signals are stacked. 

<figure style="text-align:center; display:block;" >
<img src="/assets/side_articles/deconvolution/conv_deconv.png" style="width:90%">
<figcaption>
  Figure 2. Examples for How Convolution and Deconvolution operate
</figcaption>
</figure>

</div>


### Convolution

Convolution gathers the local input signals and sums them up. Therefore, if there is a strong signal, it is propagated safely. On the other hand, the weak signals are not propagated. The convolution operation also smooths the local regions with proper strides. 


$$
\begin{aligned}
    \begin{bmatrix}
    a & b & c 
    \end{bmatrix} 
  *
    \begin{bmatrix}
    x_1 & x_2 & x_3 & x_4 & x_5
    \end{bmatrix} 
  &=
    \begin{bmatrix}
      a & b & c   \\
      & a & b & c \\
      & & a & b & c 
    \end{bmatrix}
  \cdot
    \begin{bmatrix}
    x_1 \\
    x_2 \\ 
    x_3 \\ 
    x_4 \\
    x_5
    \end{bmatrix} \\
  \\&= 
  \begin{bmatrix}
    a x_1 + b x_2 + c x_3 \\
    a x_2 + b x_3 + c x_4 \\
    a x_3 + b x_4 + c x_5
    \end{bmatrix}
\end{aligned}
$$ 


### Deconvolution 

Decovolution spreads the input signal to neighbor regions. Unlike the convolution which has preserves the signal magnitude in monotonic manner, the deconvolution can 
* Spike on a local region  
* Make checkerboard patterns

When we analyze the operation of deconv, we can see that the middle part sums three components. This difference with conv <d-footnote> Conv has equal number of components</d-footnote>, can make a biased output signal on the specific location.   

$$
\begin{aligned}
    \begin{bmatrix}
    a & b & c 
    \end{bmatrix} 
  @
    \begin{bmatrix}
    x_1 & x_2 & x_3
    \end{bmatrix} 
  &=
    \begin{bmatrix}
      a               \\
      b & a           \\
      c & b & a       \\
        & c & b       \\
        &   & c      
    \end{bmatrix}
  \cdot
    \begin{bmatrix}
    x_1 \\
    x_2 \\ 
    x_3
    \end{bmatrix} 
  \\&=
  \begin{bmatrix}
  a x_1                 \\
  b x_1 + a x_2         \\
  c x_1 + b x_2 + a x_3 \\
  c x_2 + b x_3         \\
  c x_3                 \\
  \end{bmatrix}
\end{aligned}
$$ 

When we feed an uniform input signals $[1,1,1,1,1]$, we can observe how the signals are propagated through the deconvolutional layer.  




<div class="card" style="width:auto;padding:30px;margin-top:20px" markdown="1">
<h1 style='text-align:left'> 🧙‍♂️ Problems of Deconvolution   </h1>
Assume that we feed uniform signals $[1,1,\cdots]$ <d-footnote> We assumed that all the spatial regions have same importance. Therefore, we track how the local signals are formed. I guess this idea influenced by Circuits <d-cite key="cammarata2020thread"/>.</d-footnote> . We can see two problems 
* Spike (left)
* Checkerboard (right)
<figure style="text-align:center">
<img src="/assets/side_articles/deconvolution/deconv_problem.png" style="width:100%">
<figcaption>
  Figure 3.  Problems of Deconvolution
</figcaption>
</figure>


</div>




## Resize Convolution


The checkerboard artifacts suggest that we have to handle the proper stride and kernel size for better signal propagations. However, a more simple solution is avoiding deconvolution. Instead we can we just "Convolutional Layer" for the generation parts. The underlying assumption would be

> It is more easy to compress information, then spreading information 

Therefore, the authors upsamples the original input and apply convolutional operation to decode the information in the easier way. If we follow the operation with a simple example (see the Figure below), we observe that the magnitude of an input signal is monotonically aligned with the output signal. With upscale (double) and padding for preserving the shape, we can ensure that the latent size progressively increases.



<div class="card" style="width:auto;padding:30px;margin-top:20px" markdown="1">
<h1 style='text-align:left'> 🚀 Resize Convolution   </h1>
Resize-Convolution 
Parenthesis is image size (channel, width, height)

 * → (C, H, W)  `Image` 
 * → (C, 2xH, 2xW) `Upsampling` 
 * → (C, 2xH +a , 2xW + a)  `Padding` 
 * → (C, 2xH, 2xW) `Convolution`  

<hr style='margin-top:0px;margin-bottom:0px'>
<figure style="text-align:center;margin-top:0px;">
<img src="/assets/side_articles/deconvolution/resize_conv.png" style="width:60%">
<figcaption>
  Figure 4.  Resize-Convolution example
</figcaption>
</figure>
</div>



## Experimental Results 

For experiments, the authors show that the inclusion and exclusion of deconvolutional layer  is the main reason of the checkerboard artifacts and Resize-Convolution only shows no artifacts!


<figure style="text-align:center">
<div class="row">
  <img class='column-second'  style="width: 20%;" src="https://distill.pub/2016/deconv-checkerboard/assets/fix_two_deconv_boat.png">
  <div class='column-first'>
    <h2 style='margin-top:0px;font-size:22px'>  [Resize Convs ...] - [Deconv] - [Deconv]  </h2>
    <p> When the last two layers are devons, the checkerboard patterns are visible</p>
  </div>
</div>
<div class="row">
  <img class='column-second' style="width: 20%;" src="https://distill.pub/2016/deconv-checkerboard/assets/fix_one_deconv_boat.png" >
  <div class='column-first'>
    <h2 style='margin-top:0px;font-size:22px'>  [Resize Convs ...] - [Deconv] </h2>
    <p> When the last layer is devon, the checkerboard patterns are visible. This case has higher frequency than the first one.  </p>
  </div>
</div>
<div class="row">
  <img class='column-second' style="width: 20%;"  src="https://distill.pub/2016/deconv-checkerboard/assets/fix_zero_deconv_boat.png">
  <div class='column-first'>
    <h2 style='margin-top:0px;font-size:22px' >  [Resize Convs ...]  </h2>
    <p> When we don't use deconv and replace it with Resize-Convolution, no checkerboard patterns are visible.</p>
  </div>
</div>
<figcaption>
  Figure 5.   ( © Figure from <a style="font-size:13px" href="https://distill.pub/2016/deconv-checkerboard/">the original post </a>)
</figcaption>
</figure>


## Conclusion 

In this post, we review the paper Deconvolution and Checkerboard Artifacts  [[Distill](https://distill.pub/2016/deconv-checkerboard)] which found the reasons of checkerboard artifacts and proposed solutions for it. By reviewing this paper, we found that computational link is not perfect and the inductive bias could be wrong. (That is, the inductive bias of deconvolution is not suitable for the image generation). Therefore, it is truly important to understand the operational meaning of components including "convolution, deconvolution, pooling, attention, MLP")
Many researchers have studied to investigate the reasons of artifacts and I guess there are at least four approaches

* Insufficient Training 
* Insufficient Dataset 
* Memorization and not generalization, including overfitting
* Wrong Inductive Bias (Deconvolutional Layer)

For me, a researcher in the field of interpretable AI, the most important approach is the last one. Most of tasks mostly care the training performances and do not deeply interpret the operational meaning of modules. 




## Acknowledgement 


This article is a personal review of Deconvolution and Checkboard Artifacts [[link](https://distill.pub/2016/deconv-checkerboard/)].
There could be a wrong statement as this is an **informal review** of the paper. 
