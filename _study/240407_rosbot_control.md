---
layout: share-distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-04-06
featured: true
title: 'Rosbot Control 🚗'
description: 'Rosbot Control 부분 연구'
toc:
    - name : Rosbot Control
    - name : Control Computation
    - name : Rosbot for RL
---

<iframe src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211401&authkey=!AHgcvqUNN6f39Cs" width="320" height="320" frameborder="0" scrolling="no" allowfullscreen></iframe>

* code [ktl.24.04.07](https://github.com/fxnnxc/cps_core/tree/ktl.24.04.07)

## Rosbot Control 


각 기계마다 컨트롤 해야 하는 요소 및 방법이 다르다. 강화학습으로 조작한다는 조건 하에서도 무엇을 강화학습으로 조작할 것인지 결정하기 위해서는 로봇의 작동 원리 파악이 우선시 되어야 한다. 

내가 다뤄야 하는 두 개의 로봇 유형, 로스봇과 드론은 모두 기본적으로 모터를 회전하여 움직이는 구조이며, 특히 로스봇의 경우 핸들로 방향을 전환하는 것이 아닌, 좌우 바퀴의 속도 차이를 이용하여 회전을 진행한다. 로스봇은 네 개의 바퀴를 가지고 있고 각 바퀴의 속도를 모두 조종할 수 있다. 전진의 경우 네 바퀴의 속도라 모두 동일하게 설정되어 구동한다. 만일 로봇이 휘청거리는 경우, 현상의 원인은 네 바퀴의 속도가 일정하지 않아서 생기는 문제로 보인다. 예를 들어 앞 두 바퀴의 속력을 아주 느리게, 뒷바퀴의 속력을 아주 빠르게 하는 경우에는 뒷부분이 들어올려지는 현상이 나타난다. 이러한 현상을 방지하기 위해서는 속도의 변화가 연속적이며 변화량이 크지 않아야 한다. 

로봇의 회전 방향을 계산하기 위해서는 현재 로봇이 바라보는 방향의 각도와 목표 위치 방향 각도를 계산하여, 차이만큼 회전해야 한다. 이 때 회전 방향은 각도에 따라서 시계방향(CW)과 반시계방향 (CCW)로 구성된다. 두 각도를 구했다는 가정하에 CW와 CCW의 조건은 각도 차이의 sine값이 양수인 경우와 음수인 경우로 나뉠 수 있다. 

<p align='center'>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211411&authkey=%21AMLaHKmTBpQzTSM&width=584&height=227" width="584" height="227"  />
</p>


한 가지 특이점은 방향을 고려했을 때, 각도 차이 최대는 2 Pi가 아닌 Pi라는 점이다. Pi보다 커지는 각도에 대해서는 반대방향으로 각도를 구해 회전해야 한다. 



<h2 id="control-computation"> Control Computation </h2>

* 🌸 code implementation [[github](https://github.com/fxnnxc/cps_core/blob/ktl.24.04.07/catkin_ws/src/webots_cps/controllers/Rosbot/rosbot_controllers.py)(private)]



기본적인 컨트롤러는 각도 차이에 대한 코사인 값이 1 근처에 있는 경우 전진을 하고, 멀어지게 된다면 각도를 회전해야 맞추는 방향으로 움직인다. 움직임에 대해서 결정하는 값은 두 가지이다.
* 네 바퀴에 대한 등속 $V$
* 좌우 바퀴 차이 속도 $W$ 

이 때, 고정된 파라미터들은 다음과 같다. 

* 속도 차이  $L$
* 바퀴 반지름  $r$

<p align='center'>
<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211410&authkey=%21AKVyBBXMQjCuoh8&width=471&height=158" width="471" height="158" />
</p>


추가적으로 거리가 가까울수록 속력을 낮추면 된다. 

<h2 id="rosbot-for-rl"> Towards RL adaptation for Rosbot Control </h2>

강화학습으로 컨트롤할 수 있는 것은 크게 두 가지이다. 
* 고정 파라미터 계수 조정
* V, W 값 추정 
* 모터 속도 계산 

가상-실체계 연구를 위해서는 세 가지 중에서 어떤 값을 조정하여 특정 위치로 가면 되는지 확인하는 게 중요하다. 이는 로봇이 구조적으로 선호하는 컨트롤 방식이 있으며, 더욱 효율적인 이동을 위해서 최적화하면 좋은 값을 선별할 필요가 있다는 잠을 시사한다. 

