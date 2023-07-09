---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: gym_predator_prey.bib
giscus_comments: true
disqus_comments: true
date: 2022-04-25
featured: true
toc:
  - name: Introduction
    # if a section has subsections, you can add them as follows:
    # subsections:
    #   - name: Example Child Subsection 1
    #   - name: Example Child Subsection 2
  - name: Code Examples
  - name: Basic information in Discrete environment
title: 'Gym Predator Prey Environment in Terminal'
description: 'Multi Agent Environment in Terminal'
img: assets/projects/gym_predator_prey/main.png
importance: 2
category: gym 
github : 'https://github.com/deep-ing/gym-predator-prey'
---

<div class="github-icon">
  <div class="icon" data-toggle="tooltip" title="Code Repository">
    <a href="https://github.com/deep-ing/gym-predator-prey"><i class="fab fa-github gh-icon"></i> Github</a> 
  </div> 
  {%- if project.github_stars -%}
  <span class="stars" data-toggle="tooltip" title="GitHub Stars">
    <i class="fas fa-star"></i>
    <span id="{{ project.github_stars }}-stars"></span>
  </span>
  {%- endif %}
</div>


## Introduction
Gym is the most widely used environment framework for reinforcement learning <d-cite key="brockman2016openai"></d-cite>. There are several multiagent envrionemts, but this environment has properties:
1. The meta-learning predator-prey environment with different action space
2. Rendering with python default library in terminal!



<center>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img src="https://github.com/deep-ing/gym-predator-prey/raw/main/assets/demo.gif" width=700px>
    <p> Demo of an environment with rendering! </p>
    </div>
</div>
</center>

This mini-project is reinforcement learning environment of task predator and prey. One particular interesting property is that there are 3 types of environments (discrete, continuous, and grid).<d-footnote> one hot representation, continuos, and 2d spaces </d-footnote> The environment is designed to train a model in environments with different representations but common goals. You can use different action space and observation space to represent the environment easily.


## Code Examples

<d-code block language="python">
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
</d-code>


## Basic information in Discrete environment

### Predator 🦁 
* `action_space` : {no-op, left, bottom, up, right} x [0~max_speed]
* `observation_space` : full information
* `reward` : -1 for each time step / 100 when prey is caught

###  Prey 🐔 
* `action_space` : {no-op, left, bottom, up, right} x [0~max_speed]
* `observation_space` : full information
* `reward` : 1 for each time step

###  Map 🗺 
* Grid size: [0,width] x [0, height]

<center>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/projects/gym_predator_prey/explain.png" title="example image" class="img-fluid-100 rounded z-depth-1" %}
    <p> The information on rendered screen </p>
    </div>
</div>
</center>
