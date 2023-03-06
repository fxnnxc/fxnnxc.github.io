---
layout: post
title: 'Gym Environment Information'
date: 2023-03-05 11:12:00-0400
description: gym environment
tags: RL 
categories: RL
---



###  Racing Car [Box 2D]

[gym info](https://www.gymlibrary.dev/environments/box2d/car_racing/)

* `observation space` : [96,96,3]
* `action_space` : [stiring, gas, break ] from [-1,0,0] to [1, 1, 1]
* The reward is -0.1 every frame and +1000/N for every track tile visited, where N is the total number of tiles visited in the track. For example, if you have finished in 732 frames, your reward is 1000 - 0.1*732 = 926.8 points.
* The episode finishes when all of the tiles are visited. The car can also go outside of the playfield - that is, far off the track, in which case it will receive -100 reward and die.