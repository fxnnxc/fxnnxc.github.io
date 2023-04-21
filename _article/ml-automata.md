---
layout: post
title: 'Automata and Semiautomation'
date: 2023-04-20 11:12:00-0400
description: ''
tags: Automata
category: ML
subcategory : Automata
---



Finite state machine computes the state of the variable, and provides outputs depending on the current state.  The user feed inputs in the automation, which is called a simulation and the state transition occurs. The outcome is obtained from the state of the machine. If the output is not provided, it is called a `semiautomation`. Recent work shows that RNN and Transformers can be viewed as a semiautomation where the input is a sequence and the states are also a natural language sequence. 



## Automataion




## Deterministic Finite Automation  (DFA)

A deterministic finite automation $M$ is a 5-tuple $(Q, \Sigma, T, q_0, A)$

* a finites set of states $Q$
* a finites set of input symbols $\Sigma$ (called as alphabet)
* a transition function $\delta : Q \times \Sigma \rightarrow Q$ 
* an initial state $q_0$ 
* a set of accept states $F\subset Q$


### Process of automation

The automation $M$ accepts the string $w=a_1a_2\cdots a_n$ if a sequence of states $r_0, r_1, \cdots, r_n$, exists in $Q$ with the following conditions:

1 . machine starts in the start state $r_0 = q_0$ 
2. $r_{i+1} = \delta(r_i, a_i)$
3. $r_n \in F$ 

The last condition says that the machine accepts $w$ if the last input of $w$ causes the machine to halt in one of the accepting states. Otherwise, it is said that the automaton rejects the string. 



## Semiautomataion


In mathematics and theoretical computer science, a semiautomation is a <strong> deterministic finite automation </strong> having inputs but no output. 

Components 
* a set $Q$ of states 
* a input alphabet set $\Sigma$ 
* transition function $T : \Q \times \Sigma \rightarrow Q$ 


A <strong> semiautomation </strong> is a triple $(Q, \Sigma, T)$. 
If the set of states $Q$ is a finite set, a semiautomation may be thought of as a <strong> deterministic finite automation  </strong> $(Q, \Sigma, T, q_0, A)$, but without the initial state $q_0$ or set of accept state $A$. 


