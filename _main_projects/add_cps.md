---
layout: page
title: 'CPS (Cyber Physical System)'
date: 2023-07-01
description: Project
---


<h4>Table of contents</h4>
<ul class="timeline">
<strong style='font-size:20px'> 2021</strong> 
<li><span class="badge-toc">10. Patent & Software</span> MultiRobot-Allocation Simulator</li> 
<li><span class="badge-toc">10. Paper</span> Generating Multi-agent Patrol Areas by Reinforcement Learning </li>
<li><span class="badge-toc">11. Paper</span> Analyzing Conflicting Objectives in Deep Reinforcement Learning... </li>
<li><span class="badge-toc">12. SCI Paper</span> Scheduling PID Attitude and Position Control Frequencies...</li>
<li><span class="badge-toc">12. SCI Paper</span> Cooperative Multi-Robot Task Allocation with Reinforcement Learning </li>
<li><span class="badge-toc">12. Evaluation </span> First Year Final</li>
</ul>
<ul class="timeline">
<strong style='font-size:20px'> 2022</strong> 
<li><span class="badge-toc">06. Paper</span> Automatic Analysis of Mobile Robot Decision Process...</li>

</ul>

<ul class="timeline">
<strong style='font-size:20px'> 2023</strong> 
<li><span class="badge-toc">02. Paper </span> Sample Filtering for Efficient Online Distillation...</li>
<li><span class="badge-toc">07. Software : CPS Core </span></li>
<li><span class="badge-toc">08. Paper: </span> TBD</li>
<li><span class="badge-toc">09. Evaluation </span> Second Year Mid-Term</li>
<li><span class="badge-toc">12. SCI Paper: </span> TBD</li>
<li><span class="badge-toc">12. SCI Paper: </span> TBD</li>
</ul>



## History 

--- 

### 2023.10.06.1: Rosbot Gazebo 

* 🗳️ release  : [v23.10.06.1](https://github.com/fxnnxc/add_cps/tree/v23.10.06.1)
* 👨🏻‍💻 code : see `/robots/rosbot/README.md`

로스봇 세팅 코드 클린 버전 구현 [[Document](https://github.com/fxnnxc/add_cps/tree/v23.10.06.1/robots/rosbot)]

Terminal 패널 정보:
1. roscore 
2. gazebo map load
3. Rosbot spawn
4. keyboard Control 

<img src="https://drive.google.com/uc?export=view&id=1Zw9gg6PlNxSGJ9Yfg1Xjer3j3ufiLiSL"> 


--- 

### 2023.10.06.2: Rosbot Environment Sample Collection 

* 🗳️ release  : [v23.10.06.2](https://github.com/fxnnxc/add_cps/tree/v23.10.06.2)
* 👨🏻‍💻 code : see `/labs/rosbot_collect_samples/run.py`

로스복 강화학습 환경 랜덤 액션을 통한 샘플 획득 [[Document](https://github.com/fxnnxc/add_cps/tree/v23.10.06.2/labs/rosbot_collect_samples)]

데모 비디오 [google drive](https://drive.google.com/file/d/11E2sRsafGk8bjPlP1MqxnjlAbJ52ZDmt/view?usp=sharing)
<video controls width="550">
  <source src="https://drive.google.com/uc?export=view&id=11E2sRsafGk8bjPlP1MqxnjlAbJ52ZDmt" type="video/webm" />

</video>

--- 

### 2023.10.16.1: Rosbot RPY

* 🗳️ release  : [v23.10.16.1](https://github.com/fxnnxc/add_cps/tree/v23.10.16.1)
* 👨🏻‍💻 code : see `/labs/rosbot_rpy/rosbot_rpy.py`

로봇이 놓이는 환경에서는 두 가지 좌표, world Frame / body Frame 이 존재한다. 로봇 컨트롤 시 사용자가 조종하는 컨트롤은 body frame 에서 동작하나, 실제 목표하는 위치는 world frame에 놓여있다. 따라서, 환경에서 body frame 의 목표를 world frame으로 전환하는 것은 실제 목표 위치에 도달하는 정도를 표시하기 위해 필수적이다. 이 때 계산에 관여하는 값은 `yaw` 이다. 
로봇의 실제 2차원 좌표를 $(G_x, G_y)$라고 하자, 상대적 위치 $(R_x, R_y)$의 절대좌표는 로봇의 2차원 회전 방향값 $\Psi$ (yaw)에 대해서 다음과 같이 구할 수 있다. 

$$
\begin{gather}
G_{tx}  = G_{x} +  \operatorname{Rotate}(R_{x}, R_{y}, \Psi)_x \\ 
G_{ty}  = G_{y} +  \operatorname{Rotate}(R_{x}, R_{y}, \Psi)_y
\end{gather}
$$

여기서 Rotate 는 $\Psi$ 회전행렬에 대해서 $R_{x}, R_{y}$ 값을 변환한 벡터를 반환하는 함수이다. 
로봇의 2D에서 실제 좌표에 대해서 목표로 하는 상대적 타겟 위치 (1,2) 를 다른 로봇의 yaw 값에 대해서 찍어보면 다음과 같이 구할 수 있다. 
<center>
<img src="https://drive.google.com/uc?export=view&id=14_ujsTJ3EFIQSGVYA15Xg7sr3rkMTFTR" style="width:50%">
</center>

목표로 하는 환경은 다음곽 같이 로봇의 위치 변화를 요구하는 타겟 위치가 있으며, 이에 대한 강화학습 보상은 절대 좌표계에서 주어진다. 

<center>
<img src="https://drive.google.com/uc?export=view&id=1mK_N4wltjVcj7K1sHIF63QedCtpZFQCZ" style="width:50%">
</center>


#### 시뮬레이터 Yaw값 확인 

시뮬레이터에서 Yaw 값을 확인해보면, 회전속도를 올리면 yaw 값이 선형으로 변하는 것을 확인할 수 있다. 값의 범위는 $-\pi \sim \pi$ 이다. 
<center>
<img src="https://drive.google.com/uc?export=view&id=1GUyyg8RElBJCEZvBGH5lNyTP354M-ZxX" style="width:50%">
</center>

시뮬레이터에서 Yaw 값에 해당하는 로봇의 실제 좌표는 다음과 같이 주어진다. 빨간색 좌표축을 기준으로 `yaw`값이 측정됨을 확인할 수 있다. 

<center>
<video controls width="550">
  <source src="https://drive.google.com/uc?export=view&id=1tiF-u2WAuImxhzmxmXQOltnmmUmhfK7G" type="video/webm" />
</video>
</center>


---
