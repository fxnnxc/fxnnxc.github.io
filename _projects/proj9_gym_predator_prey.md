---
layout: page
title: '🌍 #9  <br/> Terminal Gym Predator Prey'
description: '(2022.05.13) Meta learning nvironment for predator and prey'
img: assets/img/proj9/demo_image.png
importance: 2
category: work
---


There are several multiagent envrionemts, but this environment is

**(1) the meta-learning predator-prey environment with different action space**

**(2) rendering with python default library in terminal!**


<center>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/proj9/demo.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    <p> Demo of an environment with rendering! </p>
    </div>
</div>
</center>

This mini-project is reinforcement learning enviroment of task predator and prey. One particular interesting property is that there are 3 types of environments (discrete, continuous, and grid). The environment is designed to train a model in environments with different represenation but common goals. You can use different action space and observation space to represent the environment easily. 

```python
from gym_predator_prey import env_creator
env = env_creator({
    "env":"discrete",           # [grid, continuous, discrete]
    "n_predators":4,           # number of predators
    "n_preys": 10,             # number of preys
    "width":20,                # map width size 
    "height":10,               # map height size
    "predator_range":2,        # the range of predator
    "predator_max_speed":2,    # the maximum speed of predator
    "prey_max_speed" : 1       # the minimum speed of predator
})
```

### Basic information in *Discrete* environment

<p style="color:red">🦁 Predator</p>
* `action_space` : {no-op, left, bottom, up, right} x [0~max_speed]
* `observation_space` : full information
* `reward` : -1 for each time step / 100 when prey is caught

<p style="color:blue">🐔 Prey</p>

* `action_space` : {no-op, left, bottom, up, right} x [0~max_speed]
* `observation_space` : full information
* `reward` : 1 for each time step

<p style="color:green"> 🗺 Map</p>
* [0,width] x [0, height]


<center>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/proj9/explain.png" title="example image" class="img-fluid rounded z-depth-1" %}
    <p> The information on rendered screen </p>
    </div>
</div>
</center>

<hr>
You can download it in [Github](https://github.com/deep-ing/gym-predator-prey)