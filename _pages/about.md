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
 <h1 style="font-family:Times New Roman"> 🖋 Selected Papers   </h1>
<hr>
  <!--  ParchGrad: Controlled Internal Gradients for Reliable Saliency Map  -->
  <h3 class="demo-title"> ParchGrad: Controlled Internal Gradients for Reliable Saliency Map </h3>
  <div class="authors">Bumjin Park, Giyeong Jeon, and Jaesik Choi, 2023  
  </div>
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
    <div> {% include figure.html path="https://drive.google.com/uc?export=view&id=1kJwgA-XPdP0k3cd60R8H7jdqtgLbLJ44" class="img-demo" zoomable=true %} 
    </div>
  </div>
  <center>
  <div class="demolink" style='padding-bottom:1rem;padding-top:0.5rem;'>
    <a class="box-demo-link" href="" style="background:#AA00AA" >Pre-print</a> | 
      <a class="box-demo-link" href="/main_papers/parchgrad" style="background:#0005AA;" >Project</a> |
      <a class="box-demo-link" href="https://github.com/fxnnxc/Parchgrad" style="background:#000000;">Code</a> |
    <a class="tooltip-wrap">
      <span class="tooltip-span"> Abstract </span>
      <div class="tooltip-content">
      <strong> Abstract </strong> <br>
      Explaining the decision of neural networks is crucial to ensure their reliability in various image tasks. Among several input attribution methods, the saliency map explains the decision with the propagated gradients on pixels. One of the fundamental problems of the saliency map is gradient shattering which leaves noise on pixels and hinders the interpretation of attribution maps. To provide a clear explanation, controlling the internal gradient signals is essential. However, to the best of our knowledge, there is no work on modifying the internal gradient signals so that the saliency map provides reliable explanations. To initialize the control of internal gradient signals in the saliency map, we provide theoretical analysis on the controllable modification of gradients in multiple convolutional channels and propose ParchGrad, which prunes channels by partitioning multiple channels into two groups. In addition, we propose a class-wise selection method based on statistical tests to select which channels to magnify or reduce the impact of gradients. Experiments on the ImageNet1k dataset with several types of convolutional networks show that controlling the gradients with ParchGrad provides more faithful and sparse explanations.  
      </div>
    </a> 
  </div> 
    </center>

    
  
 <hr>


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


 <h2 style="font-family:Times New Roman"> 🖋 Other Papers   </h2>


<h3 class="demo-title" style='font-size:1.2rem'> Message Passing with Gating Mechanisms in Multi-agent Reinforcement Learning </h3>
<div class="demolink">
    <a class="box-demo-link" href="https://kros.org/" style="background:#B77EFA" >ICROS 2023</a> | 
    <a class="box-demo-link" href="https://drive.google.com/file/d/1icI0qQpRqa1pQPypUuVgmG7xhvlXmlQv/view?usp=sharing" >Korean Paper</a>
  <div class="authors">Bumjin Park, Cheongwoong Kang, and Jaesik Choi, 2023  </div>
</div>
<div class="row">
  <div class="column-first" style="width:75%;padding-right:10px;padding-left:20px;" >
It is important to consider the subjectivity of messages in multi-agent reinforcement learning (MARL). Based
on the assumption that the encoded information is a rather subjective information of the agent encode it, we tackle the problem of
handling objective and subjective information in MARL.
  </div>
    <div class="column-second" style="width:25%" >
  <img src="https://drive.google.com/uc?export=view&id=1RHDg7DxJ0ZwQfUUTfXeF4WSYkWntxtl3" height="80%" width="100%" style='border:1px solid #DDDDDD;border-radius:10px;' >
  </div>
</div>


<h3 class="demo-title" style='font-size:1.2rem'> Sample Filtering for Efficient Distillation in Reinforcement Learning </h3>
<div class="demolink">
   <a class="box-demo-link" href="https://kros.org/" style="background:#B77EFA" >KRoC 2022</a> | 
    <a class="box-demo-link" href="https://drive.google.com/file/d/1lcyaiEu6odTJRO2aAi8B7JyXl0nImtUm/view?usp=sharing" >Korean Paper</a> 
  <div class="authors">Bumjin Park, Cheongwoong Kang, and Jaesik Choi, 2022  </div>
</div>
<div class="row">
  <div class="column-first" style="width:60%; font-family:Times New Roman;margin-left:3%;" >
In this paper, we propose an on-line distillation framework with sample filtering
based on teacher Q-error quantiles. We evaluate the framework in control tasks and show that selecting
proper samples not only increases performance but also reduces the training time.
  </div>
  <div class="column-second" style="width:34%;margin-left:15px;" >
  <img src="https://drive.google.com/uc?export=view&id=1WLIFKnE6nH8W8F72NfdGfMCib80aD-NV" height="80%" width="100%" style='border:1px solid #DDDDDD;border-radius:10px;' >
  </div>
</div>



<h3 class="demo-title" style='font-size:1.2rem'> Automatic Analysis of Mobile Robot Decision Process with Layer-wise Relevance Propagation in Reinforcement Learning </h3>
<div class="demolink">
   <a class="box-demo-link" href="https://www.kimst.or.kr/" style="background:#B77EFA" >KIMST 2022</a> | 
    <a class="box-demo-link" href="https://drive.google.com/file/d/1p4lMXmgvg0XUn1_HuA8rutTfzjASh3XR/view?usp=share_link" >Korean Paper</a> 
  <div class="authors">Cheongwoong Kang, Bumjin Park, and Jaesik Choi, 2022  </div>
</div>
<div class="row">
  <div class="column-first" style="width:60%; font-family:Times New Roman;margin-left:3%;" >
It is challenging to explain the internal mechanisms of reinforcement learning due to the `black
box' nature of deep neural networks. Therefore, we propose an automated analysis method to understand
decision-making processes of reinforcement learning models. Specifically, we identify important input features
for a decision through layer-wise relevance propagation.
  </div>
  <div class="column-second" style="width:34%;margin-left:15px;" >
  <img src="https://drive.google.com/uc?export=view&id=1lvHF9k4bKGLmptf2hURyzJq9sSFVQRsc" height="80%" width="100%" style='border:1px solid #DDDDDD;border-radius:10px;' >
  </div>
</div>




<h3 class="demo-title" style='font-size:1.2rem'> Analyzing Conflicting Objectives in Deep Reinforcement Learning-based Target Tracking System </h3>
<div class="demolink">
   <a class="box-demo-link" href="https://www.kimst.or.kr/" style="background:#B77EFA" >KIMST 2021</a> | 
    <a class="box-demo-link" href="https://drive.google.com/file/d/1rthnTaDxhoHYM1JTmU_JDb-a43wWwrhx/view?usp=sharing" >Korean Paper</a> 
  <div class="authors">Cheongwoong Kang, Bumjin Park, and Jaesik Choi, 2021  </div>
</div>
<div class="row">
  <div class="column-first" style="width:60%; font-family:Times New Roman;margin-left:3%;" >
In this paper, we build a target tracking system based on deep reinforcement learning. Then, we analyze the trade-offs between target following and obstacle avoidance by training the model with varying weights of two objectives. We perform experiments in a virtual simulation environment. The experimental results show that the model exhibits different behaviors depending on the weights of the objectives. 
  </div>
  <div class="column-second" style="width:34%;margin-left:15px;" >
  <img src="https://drive.google.com/uc?export=view&id=1rr2OzSgbfIDFSIotOVhTBFXBGuUxFHYD" height="80%" width="100%" style='border:1px solid #DDDDDD;border-radius:10px;' >
  </div>
</div>




<h3 class="demo-title" style='font-size:1.2rem'> Generating Multi-agent Patrol Areas by Reinforcement Learning </h3>
<div class="demolink">
  <a class="box-demo-link" href="https://ieeexplore.ieee.org/abstract/document/9650047" style="background:#B77EFA" >ICCAS</a> | 
    <a class="box-demo-link" href="https://drive.google.com/file/d/1p_K0mY6WLPOWmI0pJan31tN_m8RvS6a0/view?usp=share_link" >Video</a> | 
    <a class="box-demo-link" href="https://drive.google.com/file/d/1Td24gm-56VTeKIZ64_wCYJPm67R8o3ch/view?usp=share_link" >Poster</a> 
  <div class="authors">Bumjin Park, Cheongwoong Kang, and Jaesik Choi, 2021  </div>
</div>
<div class="row">
  <div class="column-first" style="width:60%; font-family:Times New Roman;margin-left:3%;" >

We designed reinforcement learning environment for distributed patrolling agents. In the partially observable environment, the agents take actions for each one's interest and the non-stationary problem in multi-agent setting encourages the agents not to invade other agent's region. In our environment, the patrolling routes for the agents are generated implicitly.
  </div>
  <div class="column-second" style="width:34%;margin-left:15px;" >
  <img src="https://drive.google.com/uc?export=view&id=1MEWgyM_-jzjs9pBCz42i8JEuEst7djg4" height="80%" width="100%" style='border:1px solid #DDDDDD;border-radius:10px;' >
  </div>
</div>

<h3 class="demo-title" style='font-size:1.2rem'> Scheduling PID attitude and position control frequencies for time-optimal quadrotor waypoint tracking under unknown external disturbances </h3>
<div class="demolink">
  <a class="box-demo-link" href="https://www.mdpi.com/1424-8220/22/1/150" style="background:#B77EFA" >Sensors</a>
  <div class="authors">Cheongwoong Kang, Bumjin Park, and Jaesik Choi, 2021  </div>

</div>
<div class="row">
  <div class="column-first" style="width:60%; font-family:Times New Roman;margin-left:3%;" >
We suggest a method to schedule the PID position and attitude control frequencies for time-optimal quadrotor waypoint tracking. The method includes (1) a Control Frequency Agent (CFA) that finds the best control frequencies in various environments, (2) a Quadrotor Future Predictor (QFP) that predicts the next state of a quadrotor, and (3) combining the CFA and QFP for time-optimal quadrotor waypoint tracking under unknown external disturbances.
  </div>
  <div class="column-second" style="width:34%;margin-left:15px;" >
  <img src="https://www.mdpi.com/sensors/sensors-22-00150/article_deploy/html/images/sensors-22-00150-g004-550.jpg" height="80%" width="100%" style='border:1px solid #DDDDDD;border-radius:10px;' >
  </div>
</div>




<br style='margin-bottom:50px;'>

<!-- <a class="box-demo-link" href="/reading_list/" style="background:#617143 " >🐾 Research Progress</a> | 

<a class="box-demo-link" href="/reading_list/" style="background:#617143 " >🐾 Research Progress</a> |  -->


<iframe src="/assets/html/sail_research.drawio.html" width="100%" height="500px"></iframe>

<hr>
