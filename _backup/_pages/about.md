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
    <p text-align:center;> Research for All People</p>

news: true  # includes a list of news items
selected_papers: false # includes a list of papers marked as "selected={true}"
social: true  # includes social icons at the bottom of the page
---

<p style="text-align: justify;">
This blog is started by an idea that the fastest way to learn something is taking experiments.  <tag class="text-box"> Rudiment</tag> is a personal blog for experiments on various domains including <tag class="text-box">Reinforcement Learning</tag>, <tag class="text-box">Vision</tag>, <tag class="text-box">NLP</tag> and <tag class="text-box">XAI</tag>.  You can check a list of experiments in <tag href="/blog/"> Rudiment</tag>. 
If you have any idea or comments on the experiment, contact <tag class="text-box">bumjin.research@gmail.com</tag>.

</p>
 
 
 

 <h1 style="font-family:Times New Roman"> Research Topics   </h1>
 
* Explainable AI
* Mechanistic interpretability



<div style="line-height:2.0">
🅖 <a class="box-demo-link" href="https://scholar.google.co.kr/citations?user=XzIXaxoAAAAJ&hl=ko" style="background:#b4ffff; color:#000000;" >Google Scholar</a> 
<br>
🐾 <a class="box-demo-link" href="/research/" style="background:#e3ff67; color:#000000;" >Research Progress</a>  



</div>

<hr>
 <h1 style="font-family:Times New Roman"> 🖋 Papers   </h1>

  <!--  Emergence of Bias in the circuits of BERT  -->
  <h3 class="demo-title"> Emergence of Bias in the circuits of BERT </h3>
  <div class="demolink">
    <a class="box-demo-link" href="https://drive.google.com/file/d/1v3q8HBThVcIXzr0eADwiXHXB2tV2JR_m/view?usp=sharing" style="background:#617143" >Pre-print</a> | 
      <a class="box-demo-link" href="/paper/emergence_of_bias_in_bert/" style="background:#00B51E;" >Project</a>
    <div class="authors">Bumjin Park, Artyom Stitsyuk, and Jaesik Choi, 2023  </div>
  </div>
  <!--  
    <a class="box-demo-link" href="https://github.com/fxnnxc/vision-pretraining-bias" >Code</a> | 
    <a class="box-demo-link" href="/paper/explaining_pretraining_bias/"  style="background:#00B51E;">Project</a>
  -->
  <div class="row">
    <div class="column-first" style="width:80%" >
    As language models (LMs) are trained with massive data, knowledge in the model is easily biased. One way to mitigate the bias is end-to-end fine-tuning of the model. However, we hypothesize that even debiased model can have biased representation in internal blocks. To demonstrate the hypothesis, we measure the biases of internal blocks and quantitatively show "where" the bias occurs and how the debiased models still have biased knowledge. 
    </div>
    <div class="column-second" style="width:20%">
    {% include figure.html path="assets/img/paper/emergence_of_bias_in_the_circuits_of_bert.png" class="img-demo" zoomable=true %}
          <!-- <img width="100%" src="">   -->
    </div>
  </div>
  <hr/>


<!--  Pretraining Bias  -->
<h3 class="demo-title"> Explanation on Pretraining Bias of Finetuned Vision Transformer </h3>
<div class="demolink">
  <a class="box-demo-link" href="https://arxiv.org/abs/2211.15428" style="background:#617143" >Pre-print</a> | 
  <div class="authors">Bumjin Park and Jaesik Choi, 2022  </div>
</div>
<!--  
  <a class="box-demo-link" href="https://github.com/fxnnxc/vision-pretraining-bias" >Code</a> | 
  <a class="box-demo-link" href="/paper/explaining_pretraining_bias/"  style="background:#00B51E;">Project</a>
-->
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

<!--  MultiRobot Allocation Bias  -->
<h3 class="demo-title"> Cooperative Multi-Robot Task Allocation with Reinforcement Learning
 </h3>
<div class="demolink">
  <a class="box-demo-link" href="https://www.mdpi.com/2076-3417/12/1/272" style="background:#B77EFA" >Applied Sciences</a> | 
  <a class="box-demo-link" href="https://github.com/fxnnxc/Cooperative-Multi-Robot-Task-Allocation-with-Reinforcement-Learning" >Code</a> | 
  <a class="box-demo-link" href="/paper/multirobot_allocation/" style="background:#00B51E;" >Project</a>
  <div class="authors">Bumjin Park, Cheongwoong Kang, and Jaesik Choi, 2021  </div>
</div>
<div class="row">
  <div class="column-first">
This paper deals with the concept of multi-robot task allocation, referring to the assignment of multiple robots to tasks such that an objective function is maximized. The performance of existing meta-heuristic methods worsens as the number of robots or tasks increases. To tackle this problem, a novel Markov decision process formulation for multi-robot task allocation is presented for reinforcement learning. The proposed formulation sequentially allocates robots to tasks to minimize the total time taken to complete them. Additionally, we propose a deep reinforcement learning method to find the best allocation schedule for each problem. Our method adopts the cross-attention mechanism to compute the preference of robots to tasks. The experimental results show that the proposed method finds better solutions than meta-heuristic methods, especially when solving large-scale allocation problems.
  </div>
  <div class="column-second">
  {% include figure.html path="assets/img/project_imgs/multirobot-allocation.png" class="img-demo" zoomable=true %}
        <!-- <img width="100%" src="">   -->
  </div>
</div>
<hr/>


<!-- <a class="box-demo-link" href="/reading_list/" style="background:#617143 " >🐾 Research Progress</a> | 

<a class="box-demo-link" href="/reading_list/" style="background:#617143 " >🐾 Research Progress</a> |  -->
