---
layout: about
title: 🇰🇷 About Me
permalink: /
subtitle: Bumjin Park
order : 1 
profile:
  align: right
  image: me.png
  address: >
    <p text-align:center;> 🇰🇷 Korea</p>

news: true  # includes a list of news items
selected_papers: false # includes a list of papers marked as "selected={true}"
social: true  # includes social icons at the bottom of the page
---

<p style="text-align: justify;">
This blog is started by an idea that the fastest way to learn something is taking experiments.  <tag class="text-box">Creative Rudiment</tag> is a personal blog for experiments on various domains including <tag class="text-box">Reinforcement Learning</tag>, <tag class="text-box">Vision</tag>, <tag class="text-box">NLP</tag> and <tag class="text-box">XAI</tag>.  You can check a list of experiments in <tag href="/blog/"> Rudiment</tag>. 
<br/>
If you have any idea or comments on the experiment, contact <tag class="text-box">bjp032501@gmail.com</tag>.

</p>
 
 
 

 <h1 style="font-family:fantasy"> ⛰ Research Topics of Interest   </h1>
 
* 🐉 Selected Inference
* 🦾 Contextual RL
* 🐌 Knoweledge Modification
* 🌍 World Model




<div>
🅖 <a class="box-demo-link" href="https://scholar.google.co.kr/citations?user=XzIXaxoAAAAJ&hl=ko" style="background:#; color:#000000;" >Google Scholar</a> | 
🐾 <a class="box-demo-link" href="/research/" style="background:#e3ff67; color:#000000;" >Research Progress</a> | 



</div>

<hr>

 <h1 style="font-family:Cursive"> 🖋 Papers   </h1>


<!--  Pretraining Bias  -->
<h3 class="demo-title"> Explanation on Pretraining Bias of Finetuned Vision Transformer </h3>
<div class="demolink">
  <a class="box-demo-link" href="https://arxiv.org/abs/2211.15428" style="background:#617143" >Pre-print</a> | 
  <a class="box-demo-link" href="https://github.com/fxnnxc/vision-pretraining-bias" >Code</a> | 
  <a class="box-demo-link" href="https://fxnnxc.github.io/paper/explaining_pretraining_bias/" >Project</a>
  <div class="authors">Bumjin Park and Jaesik Choi, 2022  </div>
</div>

<div class="row">
  <div class="column-first">
  As the number of fine tuning of pretrained models increased, understanding the bias of pretrained model is essential. However, there is little tool to analyse transformer architecture and the interpretation of the attention maps is still challenging. To tackle the interpretability, we propose Input-Attribution and Attention Score Vector (IAV) which measures the similarity between attention map and input-attribution and shows the general trend of interpretable attention patterns. We empirically explain the pretraining bias of supervised and unsupervised pretrained ViT models, and show that each head in ViT has a specific range of agreement on the decision of the classification. We show that generalization, robustness and entropy of attention maps are not property of pretraining types. On the other hand, IAV trend can separate the pretraining types.
  
  </div>
  <div class="column-second">
  {% include figure.html path="assets/img/project_imgs/pretraining-bias.png" class="img-demo" zoomable=true %}
        <!-- <img width="100%" src="">   -->
  </div>
</div>

<hr/>


<!-- <a class="box-demo-link" href="/reading_list/" style="background:#617143 " >🐾 Research Progress</a> | 

<a class="box-demo-link" href="/reading_list/" style="background:#617143 " >🐾 Research Progress</a> |  -->
