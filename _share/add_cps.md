---
layout: share-distill
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

<img src="https://drive.google.com/uc?export=view&id=1Zw9gg6PlNxSGJ9Yfg1Xjer3j3ufiLiSL" style='width:100%'>  


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

## Rosbot Goal Control


* **관찰공간** : 로봇 상태 / 목표할당시 로봇 상태 / 상대 위치 / 목표 전달 시간
* **행동공간** : 전방속도, 좌우회전속도 (continuous)
* **보상값** : world frame의 목표 위치와 world frame의 로봇 위치 차이 
* **종료** : 1) timestep*60*control_hz 초과 / 2) 목표 거리 0.1 이하 
* **목표할당** : Reset 시 -5~5 범위의 목표위치 생성 

<d-code language='python' style='border:2px dashed gray;border-radius:10px;padding-left:5px;'>
# 관찰 공간의 각 Dimension 에 해당하는 값은 다음과 같다. 
feature_names = [  #현재 상태 
            'linear_velocity_1',
            'linear_velocity_2',
            'linear_velocity_3',
            'linear_acceleration_1',
            'linear_acceleration_2',
            'linear_acceleration_3',
            'raw',
            'pitch',
            'yaw',
            'angular_velocity_1',
            'angular_velocity_2',
            'angular_velocity_3',
        ]  + [  # 목표 할당시 상태 
            'prev_linear_velocity_1',
            'prev_linear_velocity_2',
            'prev_linear_velocity_3',
            'prev_linear_acceleration_1',
            'prev_linear_acceleration_2',
            'prev_linear_acceleration_3',
            'prev_raw',
            'prev_pitch',
            'prev_yaw',
            'prev_angular_velocity_1',
            'prev_angular_velocity_2',
            'prev_angular_velocity_3',
        ] + [ # 목표 상대 위치 정보
            'rel_x', 
            'rel_y',
            'duration'
        ]
        
</d-code>

## SAC Training Results 2023.10.17

* 🗳️ code release: [v23.10.17.1](https://github.com/fxnnxc/add_cps/tree/v23.10.17.1)
* 🔗 300K SAC 학습모델 / 분석 그림들 [link](https://drive.google.com/drive/folders/1pexb0a5R9WzmyJXHpyNv47KKprHUfNHV?usp=drive_link)

`rosbot_goal_control` 환경에 SAC으로 300K 시간에 대해서 학습한 모델의 구조는 다음과 같다. 
Actor 와 Critic은 서로 파라미터를 공유하지 않으며, Critic 의 값은 (관찰값과 행동)에 대해서 $V$ 를 반환한다. 
 


<d-code language="python" style='border:2px dashed gray;border-radius:10px;padding-left:5px;'>
Actor(
  (mlp): Sequential(
    (0): Linear(in_features=27, out_features=64, bias=True)
    (1): Tanh()
    (2): Linear(in_features=64, out_features=64, bias=True)
    (3): Tanh()
    (4): Linear(in_features=64, out_features=64, bias=True)
    (5): Tanh()
  )
  (fc_mean): Linear(in_features=64, out_features=2, bias=True)
  (fc_logstd): Linear(in_features=64, out_features=2, bias=True)
)
Critic(
  (mlp): Sequential(
    (0): Linear(in_features=29, out_features=64, bias=True)
    (1): GELU(approximate='none')
    (2): Linear(in_features=64, out_features=64, bias=True)
    (3): GELU(approximate='none')
    (4): Linear(in_features=64, out_features=1, bias=True)
  )
)
</d-code>

학습 과정에서 episode의 return 값은 다음과 같다. 학습결과 100K 수준에서 보상이 수렴한 것을 확인할 수 있다. 
성능이 -30 이후로 성능이 개선되지 않았는데, 이에 대해서 Upperbound 여부를 확인해봐야 한다.


<center>
<img src="https://drive.google.com/uc?export=view&id=15D4Izk8FA_Sl0sY-x_S5vSXin8OgnpLX" style="width:80%">
</center>

---

로봇의 컨트롤 $action = (\beta, \alpha)$ 에 대해서 $\beta$는 전진하는 속도, $\alpha$는 회전 값으로 범위는 다음과 같다. $\beta \in [0,1]$ 과 $\alpha \in (-1, 1)$ 회전값은 $-1$ 의 경우 왼쪽, $+1$의 경우 오른쪽으로 나타낸다. 학습 완료된 모델에 대해서 목표로 하는 상대위치 $(rx,ry)$ 를 줬을 때 로봇의 행동을 분석해 보면 다음과 같다. 

* 입력값: $[0,0,\cdots,0, rx,ry,0]$ 
* 출력 : $(\beta, \alpha)$

<center>
<img src="https://drive.google.com/uc?export=view&id=1_8nnH7TRBk-YsR5Tpssn7YBBYnOPPDER" style="width:49%">
<img src="https://drive.google.com/uc?export=view&id=17Ui29wMO2AJY2Dvn9Y6JOb0sFoNa8yZ0" style="width:49%">
</center>

*  회전값 $\alpha$가 y축을 기준으로 왼쪽과 오른쪽에 대해서 극단적인 회전으로 구분되는 것을 확인할 수 있다. 
* 가속도 $\beta$는 전방 좌우에 대해서 1값으로 높은 것을 볼 수 있으며, (-2, 2) 부근에서는 상대적으로 낮은 속도를 내는 것을 확인한다. 이는 학습이 불충분하게 되었기 때문으로 사료된다. 

로봇의 실제 방향 $(dx,dy)$의 값을 $(\beta, \alpha)$ 로부터 다음과 같은 식으로 유도될 수 있다. 

$$
\begin{gather}
\theta = (1-\alpha)/2 \pi \\
dx = \beta \cdot \cos{\theta}  \\
dy = \beta \cdot \sin{\theta}
\end{gather}
$$ 

방향벡터를 목표 위치 $(rx, ry) \in [-3,3] \times [-3,3]$ 의 위치에서 표시해보면 다음과 같은 로봇의 이동 선호도를 볼 수 있다.  

<center>
<img src="https://drive.google.com/uc?export=view&id=1g7BEhpLTit7VEp4991lxrvJmVnfLNuF3" style="width:80%">
</center>

---

### 2023.10.17 : 문제점 : Real Trajectory 가 완벽하지 않음. 

* Reward 가 제대로된 것과 별개로 모든 상태에 대해서 제대로 동작하는지 확실하지 않음. 
* Real Trajectory 가 더욱 제대로 되도록 학습되어야 함. 
* (**필요**) 학습 데이터를 더욱 일반화가 되도록 다양화 (입력의 속도/각도 등에 대해서 다양한 상태에서 시작해야 함. )

<center>
<img src="https://drive.google.com/uc?export=view&id=1GV5Xc5smpryb4KXlCtjDpLWeLsCJ8wMM" style="width:50%">
</center>
* 해당 그림은 CPU에서 학습된 모델로, Return 값이 좀더 낮긴 했음


---


