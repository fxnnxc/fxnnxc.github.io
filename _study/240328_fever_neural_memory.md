---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-03-28
featured: true
title: 'Learning Fever dataset '
description: '추가적인 메모리는 도움이 되는가?'
---

## Does Adaptation of Neural Memory helps the factual prediction? 



#### Fever 99 Pages F1 Score (pretrained)

|Model       |  (zero) | 1-shot  | 2-shot  | 3-shot  | 4-shot  |
|------------|---------|---------|---------|---------|---------|
|Llama2 7b   | 0.044     | **0.468** | 0.394      | 0.404      | 0.414      | 0.404      |  
|Llama2 13b  | 0.030     | 0.460     | **0.430**  | **0.448**  | **0.494**  | **0.496**  |
|Pythia 70m  | 0.000     | 0.000     | 0.000      | 0.000      | 0.000      | 0.000      |
|Pythia 160m | 0.000     | 0.000     | 0.000      | 0.000      | 0.000      | 0.000      |
|Pythia 410m | 0.000     | 0.007     | 0.000      | 0.000      | 0.000      | 0.000      |
|Pythia 1b   | 0.000     | 0.000     | 0.000      | 0.004      | 0.022      | 0.009      |
|Pythia 1.4b | 0.049     | 0.004     | 0.041      | 0.061      | 0.027      | 0.031      |
|Pythia 2.8b | 0.001     | 0.083     | 0.160      | 0.136      | 0.208      | 0.202      |
|Pythia 6.9b | **0.205** | 0.352     | 0.362      | 0.264      | 0.181      | 0.304      |
|Pythia 12b  | 0.000     | 0.382     | 0.202      | 0.147      | 0.158      | 0.184      |


#### Fever 99 Pages F1 Score (template memo)

|Model       |  (zero) | 1-shot  | 2-shot  | 3-shot  | 4-shot  |
|------------|---------|---------|---------|---------|---------|
|Llama2 7b   | 
|Llama2 13b  |
|Pythia 70m  |
|Pythia 410m |
|Pythia 1b   |
|Pythia 1.4b |
|Pythia 6.9b |
|Pythia 12b  |


#### Fever 99 Pages F1 Score (wikipage memo)

|Model       |  (zero) | 1-shot  | 2-shot  | 3-shot  | 4-shot  |
|------------|---------|---------|---------|---------|---------|
|Llama2 7b   | 
|Llama2 13b  |
|Pythia 70m  |
|Pythia 410m |
|Pythia 1b   |
|Pythia 1.4b |
|Pythia 6.9b |
|Pythia 12b  |


#### Fever 99 Pages F1 Score (+wikipage + template memo)

|Model       |  (zero) | 1-shot  | 2-shot  | 3-shot  | 4-shot  |
|------------|---------|---------|---------|---------|---------|
|Llama2 7b   | 
|Llama2 13b  |
|Pythia 70m  |
|Pythia 410m |
|Pythia 1b   |
|Pythia 1.4b |
|Pythia 6.9b |
|Pythia 12b  |


---

## Full Results 



|model        | Size |  accuracy | 	f1.micro     |	f1.macro
|-------------|------|-----------|---------------|----------------|
llama2 | 13b | 0 | 0.030 | 0.030 | 0.038 | 
llama2 | 13b | 1 | 0.460 | 0.460 | 0.306 | 
llama2 | 13b | 2 | 0.430 | 0.430 | 0.227 | 
llama2 | 13b | 3 | 0.448 | 0.448 | 0.243 | 
llama2 | 13b | 4 | 0.494 | 0.494 | 0.270 | 
llama2 | 13b | 5 | 0.496 | 0.496 | 0.276 | 
llama2 | 7b | 0 | 0.044 | 0.044 | 0.053 | 
llama2 | 7b | 1 | 0.468 | 0.468 | 0.332 | 
llama2 | 7b | 2 | 0.394 | 0.394 | 0.228 | 
llama2 | 7b | 3 | 0.404 | 0.404 | 0.232 | 
llama2 | 7b | 4 | 0.414 | 0.414 | 0.238 | 
llama2 | 7b | 5 | 0.404 | 0.404 | 0.236 | 
pythia | 12b | 0 | 0.000 | 0.000 | 0.000 | 
pythia | 12b | 1 | 0.382 | 0.382 | 0.202 | 
pythia | 12b | 2 | 0.202 | 0.202 | 0.141 | 
pythia | 12b | 3 | 0.147 | 0.147 | 0.100 | 
pythia | 12b | 4 | 0.158 | 0.158 | 0.109 | 
pythia | 12b | 5 | 0.184 | 0.184 | 0.121 | 
pythia | 6.9b | 0 | 0.205 | 0.205 | 0.144 | 
pythia | 6.9b | 1 | 0.352 | 0.352 | 0.203 | 
pythia | 6.9b | 2 | 0.362 | 0.362 | 0.280 | 
pythia | 6.9b | 3 | 0.264 | 0.264 | 0.179 | 
pythia | 6.9b | 4 | 0.181 | 0.181 | 0.136 | 
pythia | 6.9b | 5 | 0.304 | 0.304 | 0.192 | 
pythia | 2.8b | 0 | 0.001 | 0.001 | 0.002 | 
pythia | 2.8b | 1 | 0.083 | 0.083 | 0.070 | 
pythia | 2.8b | 2 | 0.160 | 0.160 | 0.104 | 
pythia | 2.8b | 3 | 0.136 | 0.136 | 0.107 | 
pythia | 2.8b | 4 | 0.208 | 0.208 | 0.147 | 
pythia | 2.8b | 5 | 0.202 | 0.202 | 0.141 | 
pythia | 1.4b | 0 | 0.049 | 0.049 | 0.055 | 
pythia | 1.4b | 1 | 0.004 | 0.004 | 0.003 | 
pythia | 1.4b | 2 | 0.041 | 0.041 | 0.037 | 
pythia | 1.4b | 3 | 0.061 | 0.061 | 0.057 | 
pythia | 1.4b | 4 | 0.027 | 0.027 | 0.036 | 
pythia | 1.4b | 5 | 0.031 | 0.031 | 0.041 | 
pythia | 1b | 0 | 0.000 | 0.000 | 0.000 | 
pythia | 1b | 1 | 0.000 | 0.000 | 0.000 | 
pythia | 1b | 2 | 0.000 | 0.000 | 0.000 | 
pythia | 1b | 3 | 0.004 | 0.004 | 0.004 | 
pythia | 1b | 4 | 0.022 | 0.022 | 0.022 | 
pythia | 1b | 5 | 0.009 | 0.009 | 0.008 | 
pythia | 410m | 0 | 0.000 | 0.000 | 0.000 | 
pythia | 410m | 1 | 0.007 | 0.007 | 0.006 | 
pythia | 410m | 2 | 0.000 | 0.000 | 0.000 | 
pythia | 410m | 3 | 0.000 | 0.000 | 0.000 | 
pythia | 410m | 4 | 0.000 | 0.000 | 0.000 | 
pythia | 410m | 5 | 0.000 | 0.000 | 0.000 | 
pythia | 160m | 0 | 0.000 | 0.000 | 0.000 | 
pythia | 160m | 1 | 0.000 | 0.000 | 0.000 | 
pythia | 160m | 2 | 0.000 | 0.000 | 0.000 | 
pythia | 160m | 3 | 0.000 | 0.000 | 0.000 | 
pythia | 160m | 4 | 0.000 | 0.000 | 0.000 | 
pythia | 160m | 5 | 0.000 | 0.000 | 0.000 | 
pythia | 70m | 0 | 0.000 | 0.000 | 0.000 | 
pythia | 70m | 1 | 0.000 | 0.000 | 0.000 | 
pythia | 70m | 2 | 0.000 | 0.000 | 0.000 | 
pythia | 70m | 3 | 0.000 | 0.000 | 0.000 | 
pythia | 70m | 4 | 0.000 | 0.000 | 0.000 | 
pythia | 70m | 5 | 0.000 | 0.000 | 0.000 | 

