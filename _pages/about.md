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
My research is focused on the understanding of mechanisms of Artificial Neural Networks motivated by the cognitive mechanism of humans. 
Feel free to contact for discussions on discovering the secrets of ANNs 🤗.
</p>

<div style="line-height:2.0">
📨 <tag class="box-demo-link" style='color:#000000;background:#ffffff;border-radius: 10px;'>bumjin.research@gmail.com</tag> 
| 📨 <tag class="box-demo-link" style='color:#000000;background:#ffffff;border-radius: 10px;'>bumjin@kaist.ac.kr</tag> 
| 🅖 <a class="box-demo-link" href="https://scholar.google.co.kr/citations?user=XzIXaxoAAAAJ&hl=ko" >Google Scholar</a> 
| 🧾 <a class="box-demo-link" href="">  CV (not available now) </a> 
| 
</div>
<hr style='margin-bottom:50px;'>

<center>
<h1 style='font-weight: 800;'> Research Products </h1>
<hr>
<div class="card" style="width:auto;padding:30px;margin-top:20px;width:50%;">
<h2 style='text-align:left'> 🎓 Research Topic Keywords   </h2>
<!-- <hr style="margin:2px;padding:2px"> -->
<hr style='margin-top:0px'>
<li style="list-style-type: none;"> <text class="box-demo-link" style="color:#000000;background:#ffffff;font-size:16px;"> Interpretable AI </text> </li> 
<li style="list-style-type: none;"> <text class="box-demo-link" style="color:#000000;background:#ffffff;font-size:16px"> Explainable AI </text> </li>
<li style="list-style-type: none;"> <text class="box-demo-link" style="color:#000000;background:#ffffff;font-size:16px"> + Mechanistic interpretability </text> </li>
<br>

<h2> 🚀 Mission </h2>
<hr style='margin-top:0px'>
</div>


<p>
Products from <strong> main research topics (M-) </strong> for my main research topics. <br>
There are also products from other fields in <strong>Side research topics (S-) </strong>.
</p>


<li> 
📌 <a class="box-demo-link" href="/main_papers/" style="background:#ffbbee; color:#000000;" > M-Papers</a> (# 1) / 
🧢 <a class="box-demo-link" href="/side_papers/" style="background:#ffbbee; color:#000000;" > S-Papers</a>  (# 1) 
</li>
<li> 
📌 <a class="box-demo-link" href="/main_projects/" style="background:#e3ff67; color:#000000;" > M-Projects</a> (# 0) / 
🧢 <a class="box-demo-link" href="/side_projects/" style="background:#e3ff67; color:#000000;" >S-Projects</a> (# 1)  
</li> 
<li>  
📌 <a class="box-demo-link" href="/main_articles/" style="background:#ddeeff; color:#000000;" >M-Articles</a> (# 0) / 
🧢 <a class="box-demo-link" href="/side_articles/" style="background:#ddeeff; color:#000000;" >S-Articles</a>  (# 3)
</li>

</center>


<hr>
 <h2 style="font-family:Times New Roman"> 🖋 Selected Papers   </h2>

  <!--  Emergence of Bias in the circuits of BERT  -->
  <h3 class="demo-title"> Emergence of Bias in the circuits of BERT </h3>
  <div class="demolink">
    <a class="box-demo-link" href="https://drive.google.com/file/d/1v3q8HBThVcIXzr0eADwiXHXB2tV2JR_m/view?usp=sharing" style="background:#617143" >Pre-print</a> | 
      <a class="box-demo-link" href="/main_papers/emergence_of_bias_in_bert/" style="background:#00B51E;" >Project</a>
    <div class="authors">Bumjin Park, Artyom Stitsyuk, and Jaesik Choi, 2023  </div>
  </div>
  <!--  
    <a class="box-demo-link" href="https://github.com/fxnnxc/vision-pretraining-bias" >Code</a> | 
    <a class="box-demo-link" href="/paper/explaining_pretraining_bias/"  style="background:#00B51E;">Project</a>
  -->
  <div class="row">
    <div class="column-first" style="width:75%" >
    As language models (LMs) are trained with massive data, knowledge in the model is easily biased. One way to mitigate the bias is end-to-end fine-tuning of the model. However, we hypothesize that even debiased model can have biased representation in internal blocks. To demonstrate the hypothesis, we measure the biases of internal blocks and quantitatively show "where" the bias occurs and how the debiased models still have biased knowledge. 
    </div>
    <div class="column-second" style="width:25%">
    {% include figure.html path="/assets/main_papers/emergence-of-bias-in-bert/main.png" class="img-demo" zoomable=true %}
          <!-- <img width="100%" src="">   -->
    </div>
  </div>
  <hr/>


<!--  MultiRobot Allocation Bias  -->
<h3 class="demo-title"> Cooperative Multi-Robot Task Allocation with Reinforcement Learning </h3>
<div class="demolink">
  <a class="box-demo-link" href="https://www.mdpi.com/2076-3417/12/1/272" style="background:#B77EFA" >Applied Sciences</a> | 
  <a class="box-demo-link" href="https://github.com/fxnnxc/Cooperative-Multi-Robot-Task-Allocation-with-Reinforcement-Learning" >Code</a> | 
  <a class="box-demo-link" href="/side_papers/multirobot_allocation/" style="background:#00B51E;" >Project</a>
  <div class="authors">Bumjin Park, Cheongwoong Kang, and Jaesik Choi, 2021  </div>
</div>
<div class="row">
  <div class="column-first" style="width:75%" >
This paper deals with the concept of multi-robot task allocation, referring to the assignment of multiple robots to tasks such that an objective function is maximized. The performance of existing meta-heuristic methods worsens as the number of robots or tasks increases. To tackle this problem, a novel Markov decision process formulation for multi-robot task allocation is presented for reinforcement learning. 
  </div>
  <div class="column-second" style="width:25%" >
  {% include figure.html path="assets/side_papers/multirobot-allocation/img5.png" class="img-demo" zoomable=true %}
        <!-- <img width="100%" src="">   -->
  </div>
</div>
<hr/>

<br style='margin-bottom:50px;'>

<!-- <a class="box-demo-link" href="/reading_list/" style="background:#617143 " >🐾 Research Progress</a> | 

<a class="box-demo-link" href="/reading_list/" style="background:#617143 " >🐾 Research Progress</a> |  -->
