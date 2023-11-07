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


<img src="https://drive.google.com/uc?export=view&id=16PSyOaKjSIvgYLotuIeVeFTdWPmHqIg_" style='width:100%'>  


<img src="https://drive.google.com/uc?export=view&id=1bWnAGL3xnr-5Xg1Uk4Z9Yy7FcjO3fLCB" style='width:100%'>  




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

<div style='border:1px solid #DDDDDD;padding:10px; border-radius:10px;' >
</d-code>
</div>

### Direct step in environment for rendering 

* masking sampling is required when sampling actions (for listener_v4)
* observation requires zero padding when computing 
* input of environment is a list of discrete  actions. 

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