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
---
 
### simple adversary
* <span style="background-color:#FFBBBB;padding:5px;font-weight:700;border-radius:10px;"> adversary </span>
* agents= [adversary_0, agent_0,agent_1]
<center>
<img src="https://pettingzoo.farama.org/_images/mpe_simple_adversary.gif" style="width:200px; border:1px solid #000000;">
</center>

#### Trained Model's Return
<img src="https://drive.google.com/uc?export=view&id=1bDbSMhWvSzNv8aZH8WRPjaC-vmNKyyns" style='width:100%;border:1px solid #000000'>

---

## simple push 
* <span style="background-color:#FFBBBB;padding:5px;font-weight:700;border-radius:10px;"> adversary </span>
* agents= [adversary_0, agent_0]

<center>
<img src="https://pettingzoo.farama.org/_images/mpe_simple_push.gif" style="width:200px; border:1px solid #000000;">
</center>


#### Trained Model's Return
<img src="https://drive.google.com/uc?export=view&id=1Y07jVmmApRG_wMXG8aqcPs7fAAaegMqA" style='width:100%'>


---


## simple crypto 
* <span style="background-color:#FFBBBB;padding:5px;font-weight:700;border-radius:10px;"> adversary </span>
* agents= [eve_0, bob_0, alice_0]

<center>
<img src="https://pettingzoo.farama.org/_images/mpe_simple_crypto.gif" style="width:200px; border:1px solid #000000;">
</center>

#### Trained Model's Return
<img src="https://drive.google.com/uc?export=view&id=1x2z3SgHBnj-Yc4odLdU6fhHhwcG6xQlq" style='width:100%'>

---

## simple reference
* <span style="background-color:#FFBBBB;padding:5px;font-weight:700;border-radius:10px;"> adversary </span>
* agents= [adversary_0, agent_0,agent_1]

<center>
<img src="https://pettingzoo.farama.org/_images/mpe_simple_reference.gif" style="width:200px; border:1px solid #000000;">
</center>


#### Trained Model's Return

<img src="https://drive.google.com/uc?export=view&id=1znUgJ_XmLzhWmGCgpezUjK338k_mugDL" style='width:100%'>

---

## simple speaker and listener
* <span style="background-color:#BBBBFF;padding:5px;font-weight:700;border-radius:10px;"> cooperative </span>
* agents=[speaker_0, listener_0]


<center>
<img src="https://pettingzoo.farama.org/_images/mpe_simple_reference.gif" style="width:200px; border:1px solid #000000;">
</center>


#### Trained Model's Return
<img src="https://drive.google.com/uc?export=view&id=1XeYHyOJD5e3FzOk8fcOr1bJIORdMlgWB" style='width:100%'>

---

## simple spread 
* <span style="background-color:#BBBBFF;padding:5px;font-weight:700;border-radius:10px;"> cooperative </span>
* agents= [agent_0, agent_1, agent_2]

<center>
<img src="https://pettingzoo.farama.org/_images/mpe_simple_spread.gif" style="width:200px; border:1px solid #000000;">
</center>



#### Trained Model's Return
<img src="https://drive.google.com/uc?export=view&id=1TFzXTBfnQeRBY7uZ3ufIm7XhYHLp_j8w" style='width:100%'>

---

## simple tag 
* <span style="background-color:#FFBBBB;padding:5px;font-weight:700;border-radius:10px;"> adversary </span>
* predator-prey
* agents= [adversary_0, adversary_1, adversary_2, agent_0]

<center>
<img src="https://pettingzoo.farama.org/_images/mpe_simple_tag.gif" style="width:200px; border:1px solid #000000;">
</center>

#### Trained Model's Return

<img src="https://drive.google.com/uc?export=view&id=1icQXDjS2Jx_eg1FznkBXpyJHwODNK7Xd" style='width:100%'>

---

## simple communication world 
* <span style="background-color:#FFBBBB;padding:5px;font-weight:700;"> adversary </span>
* agents=[leadadversary_0, adversary_0, adversary_1, adversary_3, agent_0, agent_1]



#### Trained Model's Return

----

## Three types of models and training results 

* *written in 23.11.15* 

환경별, 모델 복잡도에 따른 메시지의 혼잡도를 분석하기 위해서 일차적으로 모델을 학습하고 성능을 평가한다. 
환경은 크게 세 가지 요소 good, adversary, obstacle 가 있으며, 이들의 개수를 설정하여 환경의 복잡도를 올릴 수 있다. 
본 실험에서는 다음 개수를 설정하여 에이전트들을 학습하였다. 

* num_adversaries : [2 3 4]
* num_goods : [1 2]
* num_obstacles : [0 2 4]

```
num_steps=128
update_epochs=4
num_layers=4
total_timesteps=1000000
hidden_dim=128 
env_max_cycles=50
seed=0
msg_activation in Sigmoid 
message_dim : 1 2 4 8 16
activation : [ReLU,  Sigmoid]
num_layers : 3
update_epochs : 4 
```


### Model  V1 

메시지를 생성하지 않고, 관찰값으로만 행동을 취한다. 

```
def step1(self, obs):
   message = None 
   return message

def step2(self, obs, messages):
   combined = obs 
   return combined

def forward(self, obs):
   message = self.step1(obs)
   combined = self.step2(obs, message)
   return combined
```

### Model V2 

메시지를 생성하고, 생성된 아군 메시지들을 결합한다.
결합하는 방법은 average pooling이다. 

```
pooled_message = torch.stack(gathered_messages, dim=0).mean(dim=0)
```

### Model  V3 

메시지를 생성하고, 생성된 아군 메시지들을 결합한다.
결합하는 방법은 query 에 대해서 attention을 취하는 방법이다. 


```
query = self.queries[group_id](agent_obs).unsqueeze(1)
gathered_messages = torch.stack(gathered_messages, dim=1)
pooled_message, scores = self.attentions[group_id](query, gathered_messages, gathered_messages)
pooled_message = pooled_message.squeeze(1)
```


학습결과 : [google drive](https://drive.google.com/drive/folders/1AlFY4NYgROSEUVSzASegM5ayxx2givJ8?usp=sharing)
학습결과 full png : [google drive](https://drive.google.com/file/d/1_HXIrsk-og67QolLVB2ucgsGexC2zkaO/view?usp=drive_link)

<center>
<img src="https://drive.google.com/uc?export=view&id=1er2X_SleblXHkehaoDr9hvw0PNSr-oVa" style="width:200px; border:1px solid #000000;">
</center>

