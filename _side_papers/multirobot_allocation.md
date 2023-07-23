---
layout: header
title: "Cooperative Multi-Robot Task Allocation with Reinforcement Learning"
description: This paper deals with the concept of multi-robot task allocation, referring to the assignment of multiple robots to tasks such that an objective function is maximized.
importance: 1
date : 2021-12-31
img: /assets/side_papers/multirobot-allocation/img2.png
---

<header style='background:#000040; text-align: center; padding:30px; width:100%;'>
<h1 style="font-family:'Open Sans', sans-serif; color:white;"> Cooperative Multi-Robot Task Allocation with Reinforcement Learning </h1>
<p style="font-family:'Open Sans', sans-serif; color:white;"> Bumjin Park, Cheongwoong Kang, and Jaesik Choi <br/>  KAIST SAIL</p>
<h3 style="font-family:'Open Sans', sans-serif; color:white;"> Journal: Applied Sciences 12.1 (2021): 272 </h3>

</header>

<div class="container mt-5">

<center>
<p style="color:blue">
<a href="https://www.mdpi.com/2076-3417/12/1/272" class="d-inline-block p-3 align-top" target="_blank">
<img height="180" width="130" src="/assets/side_papers/multirobot-allocation/img6.png" style="border:1px solid" alt="None" data-nothumb="">
<br> Paper
</a>

<a href="https://github.com/fxnnxc/Cooperative-Multi-Robot-Task-Allocation-with-Reinforcement-Learning" class="d-inline-block p-3 align-top" target="_blank">
<img height="180" width="140" src="/assets/common/github.png" style="border:1px solid" alt="ArXiv Preprint thumbnail" data-nothumb="">
<br>Github Code
<br> (Becomes private on 2023.05.13 for a confidential issue)
</a>
</p>

</center>


<h1 style='color:#005922'> Contributions  </h1>


<ul>
  <li>  We propose multi-robot task allocation framework with <strong>Mixed Integer Programming.</strong> </li>
  <li>  We propose multi-robot task allocation framework with <strong>Markov Decision Process.</strong> </li>
  <li> We empirically show the gap between two formulation is less than 1 time step.  </li>
  <li> We propose <strong>transformer-based</strong> allocation architecture. </li>
  <li> Our proposed transformer-based allocation architecture with <strong>reinforcement learning </strong> shows better performance than traditional allocation algorithms such as Genetic Algorithm and Greedy Algorithm.  </li>

</ul>



<h1 style='color:#005922'>  Problem Definition  </h1>

<p>
Consider ten rooms of different sizes in a building with five robot vacuum
cleaners ready at the battery-charging machine. In this setting, we want to allocate robots
to rooms such that all rooms are cleaned and the total elapsed time is minimized. If we
consider all possible allocations, there are $5 \cdot (10!)$ possibilities that we have to consider
for robots and permutations for ten rooms. This problem is an NP-hard problem, and
finding an optimal solution takes a long time.

<br>
However, instead of static allocation of robots, we can formulate it as a sequence of decision makings, that is, a Markov Decision Process (MDP). We want to allocate robots (actions) whenever there are ready robots during cleaning. Therefore, we formulate the problem hierarchically where the inner-environment is acting of robots and the outer-environment is handles an allocation of robots. This technique could be understood as <i> temporal abstraction </i>.
</p>


<center>
<img src="/assets/side_papers/multirobot-allocation/img1.png" width=750px>
</center>

<h1 style='color:#005922'>  Greedy Algorithm is not optimal </h1>
<p>
To solve this sequential allocation problem, one traditional algorithm would be a greedy algorithm. The Figure below shows an example where there are two tasks $T_1, T_2$ and robots $R_1, R_2$. When we allocate robots based on the workload size, robot $R_2$ must travel long distance to work on $T_1$, and must return back to $T_2$ for the next work. Therefore, in this case distance based greedy allocation is better than workload based allocation. 
</p>

<center>
<img src="/assets/side_papers/multirobot-allocation/img3.png" width=750px>
</center>

<h1 style='color:#005922'>  Allocation Performance </h1>
<p>
The Multi-robot Allocation problem is NP-hard problem and the traditional algorithm can not directly solve it. 
The traditional allocation solutions firstly generate a population of solutions and gradually update the solution for a given problem. 
Note that this task is not <strong> generalization </strong> problem, but rather finding the better solution with a limited time. For example, given a single allocation problem, find the best allocation way in 3500 seconds. 
This process requires <strong> 1) solution generation </strong> and <strong> 2) solution evaluation </strong>.  The Figure below shows the makespan (time duration to finish the problem, smaller is better. )
</p>

<ul>
  <li>  IG : iterative greedy algorithm which updates the solutions to better direction </li>
  <li>  SG : stochastic version of the iterative greedy algorithm  </li>
  <li>  RD : random solution generation </li>
  <li>  GA : genetic algorithm </li>
  <li>  PPO : RL based allocation (Ours). </li>
</ul>

<center>
<img src="/assets/side_papers/multirobot-allocation/img4.png" width=750px>
</center>




<h1 style='color:#005922'>  Transformer-based Architecture </h1>

Now, I will describe the transformer-based architecture for allocation problem. The settings are as follows 

<ul>
<li>  We have a ready robot $r_i$ of our interest.  (In Figure, $R_2$ )  </li>
<li> We have robot representations (location and states) and task representations (location and workload). </li>
<li> Cross-Attention is used to get encode the robot information and decode the probability of tasks. </li>
<li> We sample task which is maximized to return (sum of rewards in RL). </li>
</ul> 

In ths experiment, we used PPO algorithm to train the architecture. 


<center>
<img src="/assets/side_papers/multirobot-allocation/img2.png" width=750px>
</center>




<h1 style='color:#005922'>  Limitation </h1>

<p>
If we visualize the allocation sequence for 5 robots and 50 tasks, the allocation does look optimal for human eyes. Even though the RL result has better performance than traditional methods, better representations for allocation must be developed. 
</p>

<center>
<img src="/assets/side_papers/multirobot-allocation/img5.png" width=750px>
</center>





<!-- 
<iframe width="700" 
src="https://www.youtube.com/embed/tgbNymZ7vqY">
</iframe> -->



<h3 class='card-header'>  bibliography </h3>
<div class='card-block'>   
<p style="margin-left: 2em;" class="card-text clickselect">
Park, Bumjin, Cheongwoong Kang, and Jaesik Choi. "Cooperative multi-robot task allocation with reinforcement learning." Applied Sciences 12.1 (2021): 272.
</p>
</div>

<h3 class='card-header'>  bibtex </h3>
<div class='card-block'>   
<pre class='card-text clickselect'>
@article{park2021cooperative,
  title={Cooperative multi-robot task allocation with reinforcement learning},
  author={Park, Bumjin and Kang, Cheongwoong and Choi, Jaesik},
  journal={Applied Sciences},
  volume={12},
  number={1},
  pages={272},
  year={2021},
  publisher={MDPI}
}
</pre>
</div>

