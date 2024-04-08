---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-04-07
featured: true
title: 'Webots Robot RL'
description: 'Webot 시뮬레이터의 로봇에 대해서 RL의 학습, 모델 배포를 관리하는 것을 목표로 한다.'
---



## Info 

* Code []

이 연구는 위봇로봇을 강화학습으로 학습시키기 위해서 



## Code Structure


* We implement an agent by a single file so that the agent's policy used for the future work. In addition, the model is saved by the state dict. 
* Basically the agent is SAC. 
* Environment class communicates with the for the purpose of the 

```bash
--wrl
    🌸--agent.py 
    --envs
        🌸--rosbot_env.py
        🌸--drone_env.py
    --utils
        🌸--webot.py
--scripts
    🌸--train_sac.py

--notebooks 
    🌸--
```

* env_type : determines the observation and reward space for the control

##