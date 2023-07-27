---
layout: share-distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-07-25
featured: true
# toc:
#   - name: Problems
title: 'Modeling Sharing Contents in  MARL'
description: 'How can we improve the quality of messages in deep learning?'
img:
---


# Introduction 

Environment is a complex space where several agents and objects make interactions. 
In the environment, rational agents act to maximize utilities and the action is learned by reinforcement learning algorithm (RL) with a deep neural network. 
As the most of environments are non-stationary, a single agent can not achieve the maximum utility only with its observation and inferences with a network. 
However, if multiple agents can communicate with each other, a single agent can get richer information about the states of the environment by simply obtaining messages from other agents. 
However, the inferred information from other agents are made by a neural network which is sometimes vulnerable and includes the personal perspective of the agent processed the information. 
For example, even though there is a single observation, two agents can encode very different messages as parameters and modules could be different. 


Therefore, it is important to consider the subjectivity of messages in multi-agent reinforcement learning (MARL). 
Based on the assumption that the encoded information is a rather subjective information of the agent encode it, 
we tackle the problem of handling objective and subjective information in MARL. 
One limitation is that the processed information by a neural network is hardly interpreted. As the starting work of this subjectivity work, 
we tackle the most minor problem whether the subjective information is better than the objective information. 
In detail, we compare the usefulness of messages from an agent to another agent in two kinds: raw observation and the output of a neural network. 



# Preliminaries 

Before proceeding, we review the previous methods in MARL. 

> <strong style="font-style:normal">Learning Shared Q-values </strong> <br>
 **QMIX**<d-cite key="rashid2018qmix"/> learns a mixing network of Q-values which outputs the mixed Q-value which is monotonically  increasing for agent Q-values. **MADDPG**<d-cite key="lowe2017multi"/> utilizes the Q-functions conditional on broad casted information from agents and execute  agents in decentralized manner with the learned policy conditioned only on the observation of each agent.  
**MAPPO** (Multi-agent PPO)<d-cite key="yu2022surprising"/> utilizes the PPO with centralized value function inputs, while the value function of **IPPO** (Independent PPO)<d-cite key="de2020independent"/> takes an independent input.


> <strong style="font-style:normal">Communication Skills </strong> <br>
> **TarMac**<d-cite key="das2019tarmac"/> utilizes the attention mechanism between agents to spread the values between communicated agents. The recurrent hidden states outputs two components query and  [key, value] vectors.  **I2C**<d-cite key="ding2020learning"/> uses the prior network to determine whom to request a message. The difference with TarMac is the separation of communication steps. We believe I2C is discrete as due to the determination of agents to communicate. Note that TarMAC is based on Q-K communication. 

> <strong style="font-style:normal">Modeling What to Share </strong> <br>
**LToS**<d-cite key="yi2022learning"/> is a hierarchical modeling of agents (bi-level optimization) where the high-level policy distributes the reward signals to neighbors and the low-level policy control agent each. The shared information is the reward of each agent and the information can benefit the cooperative MARL. 

Note that our problem is in the field of **Communication Skills** and **Modeling What to Share**, and is orthogonal to the previous methods as we design the package style rather than communication styles. 


# Proposed Method 


Several MARL methods consider *how* to communicate between agents and make message with a deep neural network (DNN). 
As the output of the DNN automatically formed with the optimization algorithms, it is natural to think that the output of the agent is a compact information necessary to communicate between agents well. However, as the deep neural network is hardly interpreted, the passed message could be ambiguous and sometimes include errors. On the other hand, the observation of the agent is most unprocessed information which does not include the knowledge of the agent. Within the progress of the interpretation of DNN, we can find that the neurons in DNN are firing when they capture features in an observation. 

We believe that the simplicity of the observation is necessary for the message, but the message may include some errors induced from the agent.  Therefore, the receiver of the message may distinguish two information to decide whether to accept the processed information or decline it. 



<figure style="text-align:center">
<img align src="/assets/bjp/marl-what-to-share/simple_communication.png" style="width:100%">
<figcaption>
The two levels communication packages. The raw observation and the processed information of the agent are passed to the second agent. The second agent process the information with gating mechanism to determine the necessary information. 
</figcaption> 
</figure>



<figure style="text-align:center">
<img align src="/assets/bjp/marl-what-to-share/subjectivity.png" style="width:100%">
<figcaption>

</figcaption>
</figure>






# Benchmarking the Environments




# Environments 

MATE<d-cite key="pan2022mate"/> is a zero-sum cooperative MARL where cameras and targets are intra-teams communicate in each. 


<figure style="text-align:center">
<img align src="https://user-images.githubusercontent.com/16078332/130274196-9d18563d-6d42-493d-8dac-326b1924d2e3.gif" style="width:30%">
<figcaption>
© Image From   <a href="https://github.com/UnrealTracking/mate">MATE github</a>
</figcaption>
</figure>
