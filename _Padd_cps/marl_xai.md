---
layout: share-distill
title: 'MARL XAI'
date: 2023-10-17
description: Project
img : ''
---

[Google Drive Link](https://drive.google.com/drive/folders/1i5B4m3gUGa8cZuUrRBUSRoJNPALdvAjb?usp=sharing)



[Pettingzoo MPE](https://pettingzoo.farama.org/environments/mpe/)

<div style='border:1px solid #DDDDDD;padding:10px; border-radius:10px;' >
<d-code language="python">
simple_adversary_v3
simple_crypto_v3
simple_push_v3
simple_reference_v3
simple_speaker_listener_v4
simple_spread_v3
simple_tag_v3
simple_world_comm_v3
</d-code>
</div>

<center>
<img src="https://drive.google.com/uc?export=view&id=16PSyOaKjSIvgYLotuIeVeFTdWPmHqIg_" style='width:70%'>  
</center>

<center>
<img src="https://drive.google.com/uc?export=view&id=1bWnAGL3xnr-5Xg1Uk4Z9Yy7FcjO3fLCB" style='width:50%'>  
</center>



<div style='border:1px solid #DDDDDD;padding:10px; border-radius:10px;' >
<d-code language='python'>

------------------------------------
   simple_adversary_v3
 => adversary_0 :  Box(-inf, inf, (8,), float32)  /  Discrete(5)
 => agent_0 :  Box(-inf, inf, (10,), float32)  /  Discrete(5)
 => agent_1 :  Box(-inf, inf, (10,), float32)  /  Discrete(5)
 - [size] raw action: (3,)
 - [size] raw next obs, reward: (3, 10) (3,)
------------------------------------
   simple_crypto_v3
 => eve_0 :  Box(-inf, inf, (4,), float32)  /  Discrete(4)
 => bob_0 :  Box(-inf, inf, (8,), float32)  /  Discrete(4)
 => alice_0 :  Box(-inf, inf, (8,), float32)  /  Discrete(4)
 - [size] raw action: (3,)
 - [size] raw next obs, reward: (3, 8) (3,)
------------------------------------
   simple_push_v3
 => adversary_0 :  Box(-inf, inf, (8,), float32)  /  Discrete(5)
 => agent_0 :  Box(-inf, inf, (19,), float32)  /  Discrete(5)
 - [size] raw action: (2,)
 - [size] raw next obs, reward: (2, 19) (2,)
------------------------------------
   simple_reference_v3
 => agent_0 :  Box(-inf, inf, (21,), float32)  /  Discrete(50)
 => agent_1 :  Box(-inf, inf, (21,), float32)  /  Discrete(50)
 - [size] raw action: (2,)
 - [size] raw next obs, reward: (2, 21) (2,)
------------------------------------
   simple_speaker_listener_v4
 => speaker_0 :  Box(-inf, inf, (3,), float32)  /  Discrete(3)
 => listener_0 :  Box(-inf, inf, (11,), float32)  /  Discrete(5)
 - [size] raw action: (2,)
 - [size] raw next obs, reward: (2, 11) (2,)
------------------------------------
   simple_spread_v3
 => agent_0 :  Box(-inf, inf, (18,), float32)  /  Discrete(5)
 => agent_1 :  Box(-inf, inf, (18,), float32)  /  Discrete(5)
 => agent_2 :  Box(-inf, inf, (18,), float32)  /  Discrete(5)
 - [size] raw action: (3,)
 - [size] raw next obs, reward: (3, 18) (3,)
------------------------------------
   simple_tag_v3
 => adversary_0 :  Box(-inf, inf, (16,), float32)  /  Discrete(5)
 => adversary_1 :  Box(-inf, inf, (16,), float32)  /  Discrete(5)
 => adversary_2 :  Box(-inf, inf, (16,), float32)  /  Discrete(5)
 => agent_0 :  Box(-inf, inf, (14,), float32)  /  Discrete(5)
 - [size] raw action: (4,)
 - [size] raw next obs, reward: (4, 16) (4,)


</d-code>
</div>

### Direct step in environment for rendering 

* masking sampling is required when sampling actions (for listener_v4)
* observation requires zero padding when computing 
* input of environment is a list of discrete  actions. 

<div style='border:1px solid #DDDDDD;padding:10px; border-radius:10px;' >
<d-code language="bash">
------------------------
simple_adversary_v3
obs length:   [11, 7, 7] | act length:   [5, 5, 5]
input action:   [4, 3, 2]
------------------------
simple_crypto_v3
obs length:   [5, 5, 7] | act length:   [4, 4, 4]
input action:   [3, 0, 3]
------------------------
simple_push_v3
obs length:   [11, 7] | act length:   [5, 5]
input action:   [1, 1]
------------------------
simple_reference_v3
obs length:   [7, 7] | act length:   [50, 50]
input action:   [48, 31]
------------------------
simple_speaker_listener_v4
obs length:   [9, 10] | act length:   [3, 5]
input action:   [2, 3]
------------------------
simple_spread_v3
obs length:   [7, 7, 7] | act length:   [5, 5, 5]
input action:   [4, 2, 2]
------------------------
simple_tag_v3
obs length:   [11, 11, 11, 7] | act length:   [5, 5, 5, 5]
input action:   [0, 1, 2, 1]

</d-code>
</div>

---


<div style='border:1px solid #DDDDDD;padding:10px; border-radius:10px;' >
<d-code language="bash">

### Direct step in environment with Communication Model 


------------------------------------
   simple_crypto_v3
actions: tensor([[3, 1, 3]]) | obs_size: (3, 8) | termination: [0 0 0]
actions: tensor([[3, 2, 2]]) | obs_size: (3, 8) | termination: [0 0 0]
actions: tensor([[0, 1, 1]]) | obs_size: (3, 8) | termination: [0 0 0]
------------------------------------
   simple_push_v3
actions: tensor([[1, 4]]) | obs_size: (2, 19) | termination: [0 0]
actions: tensor([[3, 0]]) | obs_size: (2, 19) | termination: [0 0]
actions: tensor([[0, 2]]) | obs_size: (2, 19) | termination: [0 0]
------------------------------------
   simple_reference_v3
actions: tensor([[29, 22]]) | obs_size: (2, 21) | termination: [0 0]
actions: tensor([[17, 38]]) | obs_size: (2, 21) | termination: [0 0]
actions: tensor([[41,  7]]) | obs_size: (2, 21) | termination: [0 0]
------------------------------------
   simple_speaker_listener_v4
actions: tensor([[1, 0]]) | obs_size: (2, 11) | termination: [0 0]
actions: tensor([[1, 2]]) | obs_size: (2, 11) | termination: [0 0]
actions: tensor([[0, 1]]) | obs_size: (2, 11) | termination: [0 0]
------------------------------------
   simple_spread_v3
actions: tensor([[3, 2, 3]]) | obs_size: (3, 18) | termination: [0 0 0]
actions: tensor([[3, 2, 3]]) | obs_size: (3, 18) | termination: [0 0 0]
actions: tensor([[2, 1, 4]]) | obs_size: (3, 18) | termination: [0 0 0]
------------------------------------
   simple_tag_v3
actions: tensor([[0, 0, 4, 4]]) | obs_size: (4, 16) | termination: [0 0 0 0]
actions: tensor([[3, 1, 1, 0]]) | obs_size: (4, 16) | termination: [0 0 0 0]
actions: tensor([[0, 1, 3, 1]]) | obs_size: (4, 16) | termination: [0 0 0 0]
</d-code>
</div>

---



### simple adversary
* <span style="background-color:#FFBBBB;padding:5px;font-weight:700;border-radius:10px;"> adversary </span>
* agents= [adversary_0, agent_0,agent_1]
<center>
<img src="https://pettingzoo.farama.org/_images/mpe_simple_adversary.gif" style="width:200px; border:1px solid #000000;">
</center>

#### Trained Model's Return
<img src="https://drive.google.com/uc?export=view&id=1bDbSMhWvSzNv8aZH8WRPjaC-vmNKyyns" style='width:100%;border:1px solid #000000'>



### simple push 
* <span style="background-color:#FFBBBB;padding:5px;font-weight:700;border-radius:10px;"> adversary </span>
* agents= [adversary_0, agent_0]

#### Trained Model's Return
<img src="https://drive.google.com/uc?export=view&id=1Y07jVmmApRG_wMXG8aqcPs7fAAaegMqA" style='width:100%'>

### simple crypto 
* <span style="background-color:#FFBBBB;padding:5px;font-weight:700;border-radius:10px;"> adversary </span>
* agents= [eve_0, bob_0, alice_0]

#### Trained Model's Return
<img src="https://drive.google.com/uc?export=view&id=1x2z3SgHBnj-Yc4odLdU6fhHhwcG6xQlq" style='width:100%'>


### simple reference
* <span style="background-color:#FFBBBB;padding:5px;font-weight:700;border-radius:10px;"> adversary </span>
* agents= [adversary_0, agent_0,agent_1]

#### Trained Model's Return
<img src="https://drive.google.com/uc?export=view&id=1znUgJ_XmLzhWmGCgpezUjK338k_mugDL" style='width:100%'>


### simple speaker and listener
* <span style="background-color:#BBBBFF;padding:5px;font-weight:700;border-radius:10px;"> cooperative </span>
* agents=[speaker_0, listener_0]

#### Trained Model's Return
<img src="https://drive.google.com/uc?export=view&id=1XeYHyOJD5e3FzOk8fcOr1bJIORdMlgWB" style='width:100%'>



### simple spread 
* <span style="background-color:#BBBBFF;padding:5px;font-weight:700;border-radius:10px;"> cooperative </span>
* agents= [agent_0, agent_1, agent_2]

<img src="https://drive.google.com/uc?export=view&id=1TFzXTBfnQeRBY7uZ3ufIm7XhYHLp_j8w" style='width:100%'>


### simple tag 
* <span style="background-color:#FFBBBB;padding:5px;font-weight:700;border-radius:10px;"> adversary </span>
* predator-prey
* agents= [adversary_0, adversary_1, adversary_2, agent_0]

<img src="https://drive.google.com/uc?export=view&id=1icQXDjS2Jx_eg1FznkBXpyJHwODNK7Xd" style='width:100%'>

### simple communication world 
* <span style="background-color:#FFBBBB;padding:5px;font-weight:700;"> adversary </span>
* agents=[leadadversary_0, adversary_0, adversary_1, adversary_3, agent_0, agent_1]