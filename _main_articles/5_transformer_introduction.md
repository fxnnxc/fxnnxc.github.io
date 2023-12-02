---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
    - name: Youngju Joung
      affiliations:
        name: KAIST
    - name: Enver Menadjiev
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: false
date: 2023-09-08
featured: true
title: '2.1 Introduction to Transformer'
description: '📕 Chapter 2. GPT <br> <em>Computational Interpretation of Circuits</em> '
img: "https://drive.google.com/uc?export=view&id=1vcRdgWcqydwCs90_0c_PjLPi18LofMNa"
---

## Introduction

Interpretation of GPT is the most import task in deep learning to provide not only advanced but also reliable deep neural network. 
This article is about the interpretation of representations in GPT (transformer decoder-only model). 
Most of ideas in this post comes from [A Mathematical Framework for Transformer Circuits](https://transformer-circuits.pub/2021/framework/index.html), and we add some new thoughts to interpret the GPT. 


The topics described in this article are as follows: 

* <span style="font-weight:400;border:1px solid #FFDDDD;border-radius:10px;background-color:#FFDDDD;padding:2px;"> 1. Streams </span> : the stream of token representations over layers
* <span style="font-weight:400;border:1px solid #FFDDDD;border-radius:10px;background-color:#DDFFDD;padding:2px;"> 2. Residual Spaces </span> : communication between connected spaces. 
* <span style="font-weight:400;border:1px solid #FFDDDD;border-radius:10px;background-color:#DDDDFF;padding:2px;"> 3. Read and Write </span> : interpret residual stream as read and write modules 
* <span style="font-weight:400;border:1px solid #FFDDDD;border-radius:10px;background-color:#FFFFDD;padding:2px;"> 4. Logit Lens </span> : getting contribution of each block
* <span style="font-weight:400;border:1px solid #FFDDDD;border-radius:10px;background-color:#FFDDFF;padding:2px;"> 5. Token Communication </span> : information passing in tokens.  
* <span style="font-weight:400;border:1px solid #FFDDDD;border-radius:10px;background-color:#DDFFFF;padding:2px;"> 6. GPT Modules </span> : description of basic modules in GPT : block, MLP, ATTN 
* <span style="font-weight:400;border:1px solid #FFDDDD;border-radius:10px;background-color:#DDDDDD;padding:2px;"> 7. Implementation </span> : verify the forward passes of modules
* <span style="font-weight:400;border:1px solid #FFDDDD;border-radius:10px;background-color:#FFDEAD;padding:2px;"> 8. Interpretability </span> : find out where knowledge is stored




### GPT : Causal Language Modeling 


GPT is an autoregressive token generator. 

<figure>
<img src="https://drive.google.com/uc?export=view&id=1vcRdgWcqydwCs90_0c_PjLPi18LofMNa">
</figure>


### Simplest : Embed and Unembed 


The most simplest form of token generation is done by embedding the current token and unembed the next token.

<figure>
<img src="https://drive.google.com/uc?export=view&id=1kYZqPC5ACfpV1lk7lBjeZgxzPS_TXXfn">
</figure>


---

## 1. Streams 

<!-- Slideshow container -->
<div style="display:flex;justify-content:center; overflow: show;">
<div class="slideshow-container" style='margin-left:0rem;'>
  <!-- Full-width images with number and caption text -->
  <div class="mySlides">
    <figcaption style='text-align:center;color:AAAAFF;font-size:1.4rem;'>
    <strong> [1/3]</strong> Transformer  Architecture Overview
    </figcaption>
    <img src="https://drive.google.com/uc?export=view&id=1kmhnEEVpwv-OGNHHYtt1KMYanoyxcjRj" style="width:40rem;padding-right:5rem;padding-left:5rem;border:2px #DDDDFF solid;">
    
  </div>

  <div class="mySlides">
    <figcaption style='text-align:center;color:AAAAFF;font-size:1.4rem;'>
    <strong> [2/3]</strong> Multi Layer Perceptron Architecture Overview
    </figcaption>
    <img src="https://drive.google.com/uc?export=view&id=14EQfiVFGBlNoBEy7RXgzMfT6HoaMQDVs" style="width:40rem;padding-right:5rem;padding-left:5rem;border:2px #DDDDFF solid;">
  </div>

  <div class="mySlides">
    <figcaption style='text-align:center;color:AAAAFF;font-size:1.4rem;'>
    <strong> [3/3]</strong> Multi Head Attention Architecture Overview
    </figcaption>
    <img src="https://drive.google.com/uc?export=view&id=1zNSJL-c0tTG26YppqLggb-JpDNavhAXv" style="width:40rem;padding-right:5rem;padding-left:5rem;border:2px #DDDDFF solid;">
  </div>

  <!-- Next and previous buttons -->
  <a class="prev" onclick="plusSlides(-1)">&#10094;</a>
  <a class="next" onclick="plusSlides(1)">&#10095;</a>
    <br>
    <!-- The dots/circles -->
    <div style="text-align:center">
    <span class="dot" onclick="currentSlide(1)"></span>
    <span class="dot" onclick="currentSlide(2)"></span>
    <span class="dot" onclick="currentSlide(3)"></span>
    </div>
</div>
</div>

---


## 2. Residual Spaces

<figure >
<figcaption markdown="1">
Figure.  Residual space image is obtained from [A Mathematical Framework](https://transformer-circuits.pub/2021/framework/index.html). 
</figcaption>
<img src="https://drive.google.com/uc?export=view&id=1UQos94yTCB0JeYy1GExR0xhgKMhJa1c6">

</figure>



---

## 3. Read and Write
 
The input representation $x$ is encoded by a module $E_i:x\rightarrow r_i(x)$ and further added with the identity representation. 
The hidden representations $h_i$ where $i$ is the location of representation are as follows: 

$$ 
\begin{gather}
h_0 = x \\
h_1 = x + r_1(x)  \\ 
\Rightarrow h_2 = x + r_1(x) +  r2(x + r_1(x))
\end{gather}
$$ 


<center>
<figure >
<figcaption style='text-align:center;color:#8888FF;'>
Figure. 
</figcaption>
<img src="https://drive.google.com/uc?export=view&id=1lGBOmIDAdCi7GHbUgCl6zNQvjbu8vXmc" style="width:auto;height:30rem;border:2px #DDDDFF solid;">
</figure>
</center>

---


## 4. Logit Lens

As all the representations are linearly added, we can decompose the contribution of each block or location. 
By combining the idea that the final hidden representation is further mapped to the vocabulary, we can factorize the logits of vocabulary as follows. 


<div style="display:flex;justify-content:center; overflow: show;">
<figure style="width:70rem;">
<img src="https://drive.google.com/uc?export=view&id=1JA8OUQNXn0cHftv3804UooPlTyzdzNRc">
<figcaption style='text-align:center;color:#8888FF;'>
Figure. 
</figcaption>
</figure>
</div>



---


## 5. GPT Modules  

<!-- Slideshow container -->
<div style="display:flex;justify-content:center; overflow: show;">
<div class="slideshow-container" style='width:100rem;'>
  <!-- Full-width images with number and caption text -->
  <div class="mySlides2">
    <figcaption style='text-align:center;color:AAAAFF;font-size:1.4em;'>
    <strong> [1/3]</strong> Transformer  Architecture Overview
    </figcaption>
    <img src="https://drive.google.com/uc?export=view&id=1fSy0KqpOuG7PRGkCi8PQStuaoD2bzTa0" style="width:60rem;padding-right:5rem;padding-left:5rem;border:2px #DDDDFF solid;">
    
  </div>

  <div class="mySlides2">
    <figcaption style='text-align:center;color:AAAAFF;font-size:1.4rem;'>
    <strong> [2/3]</strong> Multi Layer Perceptron Architecture Overview
    </figcaption>
    <img src="https://drive.google.com/uc?export=view&id=19Ty1Re3adPRGtJeVlyv12e0IXtpmm6r-" style="width:60rem;padding-right:5rem;padding-left:5rem;border:2px #DDDDFF solid;">
  </div>

  <div class="mySlides2">
    <figcaption style='text-align:center;color:AAAAFF;font-size:1.4rem;'>
    <strong> [3/3]</strong> Multi Head Attention Architecture Overview
    </figcaption>
    <img src="https://drive.google.com/uc?export=view&id=1R0K5MlHI4kmFqvfwVpK45w4243GyEotX" style="width:60rem;padding-right:5rem;padding-left:5rem;border:2px #DDDDFF solid;">
  </div>

  <!-- Next and previous buttons -->
  <a class="prev" onclick="AplusSlides(-1)">&#10094;</a>
  <a class="next" onclick="AplusSlides(1)">&#10095;</a>

<br>
<!-- The dots/circles -->
<div style="text-align:center">
  <span class="dot2" onclick="AcurrentSlide(1)"></span>
  <span class="dot2" onclick="AcurrentSlide(2)"></span>
  <span class="dot2" onclick="AcurrentSlide(3)"></span>
</div>
</div>
</div>

---


## 6. Token Communication 


The residual stream only combines representations of modules in a specific token stream. In other words, there is no communication between tokens if we don't mix the representations. 
A way to mix representations in the transformer is the multi-head attention (MHA) pass information in each token weighted by attention scores. 

* All the tokens will ask other tokens "how much information to give to the query"
* Each token will give value weighted by attention score (query * key)

Note that the stream of the query token will be replaced by values of other tokens unless query get higher attention to itself. 
For example, if the values of other tokens are noise, the attention score of itself should be high to mitigate the disturbance of noise. 

<figure>
<img src="https://drive.google.com/uc?export=view&id=1ZqmEhrTnagm67jVur-zeBAhZxekMnxJA">
<figcaption markdown="1">
Figure. Residual space image is obtained from [A Mathematical Framework ](https://transformer-circuits.pub/2021/framework/index.html). 
</figcaption>
</figure>



### Multimodal Token Communication 

The token communication is essential in **text-to-image** generation such as Parti [Parti Google](https://sites.research.google/parti/)
**In multimodality, the values will be text representation which are injected to the stream of image representations.** 

<img src="https://sites.research.google/parti/assets/parti_overview.jpg" style="width:100%">


---

## 7. Implementations 


<figure style="">
<figcaption style='text-align:left;color:#8888FF;'>
👩🏻‍💻 Please click the image to see details of GPT block forwarding.
</figcaption>
<img src="https://drive.google.com/uc?export=view&id=1xCTc2ovedTFFy3luGHz8Ixik5peNJlMM" onClick="window.open(this.src)" role="button">
</figure>


<figure style="">
<figcaption style='text-align:left;color:#8888FF;'>
👨🏻‍💻 Please click the image to see details of multi head attention forwarding.
</figcaption>
<img src="https://drive.google.com/uc?export=view&id=1BA5QR8cRtOdP8bcyn2RJEEdsI1MmVS_M" onClick="window.open(this.src)" role="button">
</figure>


---


## 8. Interpretability

How do the Transformer-based models perform well? How do they store the knowledge? We need to find out how they work, and there are some methods to make interpretations of predictions.



### Knowledge Attribution

<figure>
<img alt="knowledge attribution" src="https://github.com/team-sail/team-sail.github.io/assets/63037270/9d223908-0f6f-4825-9a2a-d14616944451">
<figcaption markdown="1">
Figure. Knowledge Neurons in Pretrained Transformers [Dai et al., 2022](https://arxiv.org/pdf/2104.08696.pdf). 
</figcaption>
</figure>

The feed-forward module is regarded as a key-value memory.

$$ 
FFN(x) = f(x \cdot K^{\top})\cdot V \quad where\; K, V \in \mathbb{R}^{d_m \times d} 
$$

In this context, we can try finding out which neurons in the FFN have knowledge of a specific prediction. The **Knowledge Attribution** method allows measuring the contribution of each neuron to prediction when performing a fill-in-the-blank cloze task. The attribution score of each neuron is defined as follows. $w_i^{(l)}$ denotes the $i$-th intermediate neuron in the $l$-th FFN.

$$ 
Attr(w_i^{(l)}) = \bar{w}_i^{(l)} \int_{\alpha = 0}^{1} \frac{\partial P_x(\alpha \bar{w}_i^{(l)})}{\partial w_i^{(l)}}d\alpha 
$$

Intuitively, as $\alpha$ changes from $0$ to $1$, by integrating the gradients, $Attr(w_i^{(l)})$ accumulates the output probability change caused by the change of $w_i^{(l)}$.



### Patching

<figure>
<img width="507" alt="Path-patching" src="https://github.com/team-sail/team-sail.github.io/assets/63037270/70bcd384-05af-47ca-963a-4495c74c7283">
<figcaption markdown="1">
Figure. Patching [Wang et al., 2023](https://iclr.cc/media/PosterPDFs/ICLR%202023/11341.png?t=1682885776.8460147). 
</figcaption>
</figure>

**Patching** refers to replacing activations from a different input to identify which component causes an effect on the prediction.

<figure>
<img alt="patching" src="https://github.com/team-sail/team-sail.github.io/assets/63037270/2e30109e-4295-4043-aee8-87da4b66b9be">
<figcaption markdown="1">
Figure. Causal Traces [Meng et al., 2022](https://arxiv.org/abs/2202.05262). 
</figcaption>
</figure>

We can measure the influence of the target node by changing its (noised) activation to a clean value.

<figure>
<img alt="results" src="https://github.com/team-sail/team-sail.github.io/assets/63037270/8279343e-549e-445e-b111-b1e95d9ffa7d">
<figcaption markdown="1">
Figure. Results [Meng et al., 2022](https://arxiv.org/abs/2202.05262). 
</figcaption>
</figure>

To analyze the results, the AIE score is used as a metric. It can be computed by $Avg(P_{replaced}[o] - P_{corrupted}[o])$ where $o$ denotes the correct output. As we can see in the figure, the last token of the subject has a significant impact on the causal relationship in the early site layer. In addition, the MLP of the early site and the attention mechanism of the late site appear to play an important role in calculating the final prediction.

<script src="/assets/js/curriculum_2_1.js">
