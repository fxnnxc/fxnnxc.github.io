---
layout: header
title: ""
description:  
importance: 1
---


<header style='background:#000040; text-align: center; padding:40px; width:100%;'>
<h1 style="font-family:'Open Sans', sans-serif; color:white;"> Emergence of Bias in the circuits of BERT </h1>
<p style="font-family:'Open Sans', sans-serif; color:white;"> Bumjin Park, Artyom Stitsyuk, and Jaesik Choi <br/>  KAIST SAIL</p>
<h3 style="font-family:'Open Sans', sans-serif; color:white;"> Arxiv 2023 </h3>

</header>

<div class="container mt-5">

<br>
<h1 style='color:#000040'> 1. Introduction  </h1>
<p>
Evaluation of a pre-trained large models is essential to understand the properties of models. One of critical evaluations is the bias of a model for social groups which are also known as democratic axes. Examples are "gender, race, and community". On these democratic axes, there are words that the model should not bias to specific words. For example, when you ask a model: 
</p>
<Blockquote style='background-color:#FFF440'>
"Who can be a nurse?"
</Blockquote>
<p>
Then, the likelihoods of words for the model output would be 
</p>
<Blockquote style='background-color:#FFF490'>
LM's Answer : "woman: 0.7, man: 0.2, ..." 
</Blockquote>

<p>
Even though the word <it>"nurse"</it> is highly entangled with the word  <it>"woman"</it>, 
The man should have similar probability with the woman as he could be a nurse too. 
Under this bias problem, we provide a quantitative analysis  on the emergence of bias in the circuits of BERT. 
That is, we believe that the bias emerges at the internal representation of a model and we debias the layer to mitigate the bias. 
To this end, we propose several quantitative measures to understand the bias of BERT. 
<br>
</p>

<br>
<h2 style='color:#000040'> 
2. Bias Contribution of Layers 
</h2>
<p>
Measuring the contribution of layers is done with direct mapping of the hidden representations to the vocabulary. The direct mapping is possible as the blocks are connected by residual streams which refine the hidden representations. 
Figure below shows how logit-lens are related to the bias analysis. 
At the mask location, the prediction depends on the word <it> Nurse </it> which triggers bias of outputs. 
Note that every block outputs can be directly mapped to the vocabulary. 
</p>

<center>
<img src="/assets/img/paper/emergence-of-bias-in-bert/main.png" width=900px>
</center>

<br>
<h2 style='color:#000040'> 
3. Measuring entropy of logits
</h2>
<p>
To quantitatively measure the bias (imbalance of logits), we use entropy of probabilities over the democratic words such as $\{he, she, woman, man\}$ for <it> Gender </it> democratic axis.  Figure below shows the entropy over attention modules and MLP modules over all layers. 
We observe that the emergences of imbalance are quite different for each democratic axis. 
In addition, MLP on [MASK] token has imbalance first than ATTN. 
</p>
<center>
<img src="/assets/img/paper/emergence-of-bias-in-bert/entropy.png" width=900px>
</center>

<br>
<h2 style='color:#000040'> 
4. Masked Location Bias Scores
</h2>
<p>
To blame the bias on the [MASK] token, we should verify whether the [MASK] position makes the bias or obtained the bias from other tokens. To measure the responsibility, we propose  Masked Location Bias Scores (MLBs) which measures the difference of entropies of logit lens for MLP and ATTN at a block. 
Figure below shows the responsibility of bias. 
</p>
<center>
<img src="/assets/img/paper/emergence-of-bias-in-bert/mlbs.png" width=400px>
</center>
<p>
Figure below shows the MLBs over all layers. As color is close to red, the [MASK] location produces high bias than the bias obtained from other tokens. On the other hand, if the color is close to blue, the [MASK] location mitigate the bias. 
</p>
<center>
<img src="/assets/img/paper/emergence-of-bias-in-bert/mlbs_results.png" width=900px>
</center>


<br>
<h2 style='color:#000040'> 
5. What happens when debias?
</h2>
<p>
We conjecture that the bias of logit-lens for the selected layer only debias locally. That is, the logits of other layers are still biased. 
Figure below shows the marginal contributions of layers before debias and after debias. Note that when we select the debias location early, the logits of upper layers are still remaining.  On the other hand, the debias of upper layers results in the survival of biases in the lower layers. 
One important observation is that they are sum to zero and the final output is also zero. 
Therefore, the observation (prediction) is debiased, while the internal layers still have biases. 
</p>
<center>
<img src="/assets/img/paper/emergence-of-bias-in-bert/debiased.png" width=900px>
</center>


<br>
<h2 style='color:#000040'> 
Conclusion
</h2>
<p>
We propose several quantitative analysis to understand biases in internal layers of BERT. 
We believe that understanding of internal biases is important to perfectly mitigate the bias of large models. 
</p>

