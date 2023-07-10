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
    <p text-align:center;> AI Researcher </p>

news: true  # includes a list of news items
selected_papers: false # includes a list of papers marked as "selected={true}"
social: true  # includes social icons at the bottom of the page
---

<p style="text-align: justify;">
Hi, my name is Bumjin Park, a Ph.D student in <a href="https://gsai.kaist.ac.kr/">the Graduate School of AI</a> in <a href="https://www.kaist.ac.kr/en/">KAIST</a>.  
My research is focused on the understanding of mechanisms of Artificial Neural Networks. 
I'm working to discover the secrets of ANNs.
</p>

<div style="line-height:2.0">
📨 <tag class="box-demo-link" style='color:#000000;background:#ffffff;border-radius: 10px;'>bjp032501@gmail.com</tag> 
| 📨 <tag class="box-demo-link" style='color:#000000;background:#ffffff;border-radius: 10px;'>bumjin@kaist.ac.kr</tag> 
| 🅖 <a class="box-demo-link" href="https://scholar.google.co.kr/citations?user=XzIXaxoAAAAJ&hl=ko" >Google Scholar</a> 
| 🧾 <a class="box-demo-link" href="">  CV (not available now) </a> 
| 
</div>


<hr>
<div class="card" style="width:auto;padding:30px;margin-top:20px">
<h1 style='text-align:left'> 🎓 Research Topics   </h1>
<!-- <hr style="margin:2px;padding:2px"> -->
<hr style='margin-top:0px'>
<li style="list-style-type: none;"> <text class="box-demo-link" style="color:#000000;background:#ffffff;font-size:16px"> Mechanistic interpretability </text> </li>
<li style="list-style-type: none;"> <text class="box-demo-link" style="color:#000000;background:#ffffff;font-size:16px"> Interpretable AI </text> </li>
<li style="list-style-type: none;"> <text class="box-demo-link" style="color:#000000;background:#ffffff;font-size:16px"> Explainable AI </text> </li>
<br>
<h1> 🤗 Mission </h1>
<hr style='margin-top:0px'>


</div>

<hr>

<h3> Research Products </h3>
<p>
These are main results of my research related to the research topics. 
</p>


<li><a class="box-demo-link" href="/main_papers/" style="background:#ffbbee; color:#000000;" >📌 Papers</a> (#0)
</li>
<li>  <a class="box-demo-link" href="/main_projects/" style="background:#e3ff67; color:#000000;" > 📌 Projects</a> (#0)
</li> 
<li>  <a class="box-demo-link" href="/main_articles/" style="background:#ddeeff; color:#000000;" >📌 Articles </a> (#0) 
</li>

<p>
Although, my major interests are interpretable AI, there are products from other fields. Here are the Lists of materials related to side research topics: 
<br>
<a class="box-demo-link" href="/side_papers/" style="background:#ffddee; color:#000000;" >☕️ S-Papers (#0) </a>  
 <a class="box-demo-link" href="/side_projects/" style="background:#eeffee; color:#000000;" >☕️ S-Projects (#1)</a>   
 <a class="box-demo-link" href="/side_articles/" style="background:#eeeeff; color:#000000;" >☕️ S-Articles (#0) </a> .
</p>

<hr>
 <h1 style="font-family:Times New Roman"> 🖋 Selected Papers   </h1>

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
