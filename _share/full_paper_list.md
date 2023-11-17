---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
date: 2023-11-10
featured: true
# toc:
#   - name: Problems
title: 'Full Paper List'
description: ''

---

### 2024

### 2023 

* ParchGrad :

* Source Identification Problem : 

* 멀티 에이전트 강화학습에서의 게이트 기반 메시지 교환. 제어.로봇.시스템학회 논문지, 29(11), 847-853. **박범진**, 강천웅 and 최재식. [[journal](https://www.kci.go.kr/kciportal/ci/sereArticleSearch/ciSereArtiView.kci?sereArticleSearchBean.artiId=ART003013340)] [[paper](https://drive.google.com/file/d/1icI0qQpRqa1pQPypUuVgmG7xhvlXmlQv/view)]

### 2022

* Explanation on Pretraining Bias of Finetuned Vision Transformer. **Bumjin Park** and Jaesik Choi. arXiv preprint arXiv:2211.15428 (2022).

### 2021

* Cooperative multi-robot task allocation with reinforcement learning, **Bumjin Park**, Cheongwoong Kang, and Jaesik Choi. Applied Sciences 12.1 (2021): 272.

* Generating Multi-agent Patrol Areas by Reinforcement Learning, **Bumjin, Park**, Cheongwoong Kang, and Jaesik Choi.  2021 21st International Conference on Control, Automation and Systems (ICCAS). IEEE, 2021.

## NLP 

<h3 style="padding-bottom:0px;margin-bottom:0px;"> Emergence of Bias in the circuits of BERT </h3>
<i> <strong>Bumjin Park</strong>, Artyom Stitsyuk, and Jaesik Choi </i>
<div> <a class="box-demo-link" href="https://drive.google.com/file/d/1v3q8HBThVcIXzr0eADwiXHXB2tV2JR_m/view?usp=sharing" style="background:#617143" >Pre-print</a> 
<a class="box-demo-link" href="/main_papers/emergence_of_bias/" style="background:#0000FF;" >Project</a><br>
<span class="box-demo-link"  style="background:#000000;"><i>NLP, XAI</i></span>
<span class="box-demo-link"  style="background:#000000;" ><i>2023</i></span>
 <span class="tooltip-wrap">
      <span class="tooltip-span"> Abstract </span>
      <div class="tooltip-content">
      <strong> Abstract </strong> <br>
      As language models (LMs) are trained with massive data, knowledge in the model is easily biased. One way to mitigate the bias is end-to-end fine-tuning of the model. However, we hypothesize that even debiased model can have biased representation in internal blocks. To demonstrate the hypothesis, we measure the biases of internal blocks and quantitatively show "where" the bias occurs and how the debiased models still have biased knowledge. 
      </div>
    </span> 
<p> This work provides quantitative analysis on the bias circuits. </p>
</div>
<img src="/assets/main_papers/emergence-of-bias-in-bert/main.png" style="width:100%; padding:1rem; border:1px #AAAAFF solid;border-radius:10px;">

---

## MARL

<h3 style="padding-bottom:0px;margin-bottom:0px;"> Cooperative Multi-Robot Task Allocation with Reinforcement Learning </h3>
<i> <strong>Bumjin Park</strong>, Cheongwoong Kang, and Jaesik Choi </i>
<div>
<a class="box-demo-link" href="https://www.mdpi.com/2076-3417/12/1/272" style="background:#B77EFA" >Applied Sciences</a> 
<a class="box-demo-link" href="https://github.com/fxnnxc/Cooperative-Multi-Robot-Task-Allocation-with-Reinforcement-Learning" style="background:#FF0000;" >Code</a> 
<a class="box-demo-link" href="/side_papers/multirobot_allocation/" style="background:#0000FF;" >Project</a> <br>
<span class="box-demo-link"  style="background:#000000;" > <i>MARL, Cooperative</i></span>
<span class="box-demo-link"  style="background:#000000;" > <i>2021</i></span>
 <span class="tooltip-wrap">
      <span class="tooltip-span"> Abstract </span>
      <div class="tooltip-content">
      <strong> Abstract </strong> <br>
      This paper deals with the concept of multi-robot task allocation, referring to the assignment of multiple robots to tasks such that an objective function is maximized. The performance of existing meta-heuristic methods worsens as the number of robots or tasks increases. To tackle this problem, a novel Markov decision process formulation for multi-robot task allocation is presented for reinforcement learning. 
      </div>
    </span> 
<p> This work provides mixed integer programming formulation and transformer allocations. </p>
</div>
<img src="/assets/side_papers/multirobot-allocation/img5.png" style="width:100%; padding:1rem; border:1px #AAAAFF solid;border-radius:10px;">

## RL 

 <hr>


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