---
layout: share-distill
title: '[MARL-Message-Adaptor] 2. Framework '
date: 2024-02-01
description: Project
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
img : ''
---

## Four Message Types 


## Combination of Message Types  

index | obs | action | probs | hidden | 
|  --- | --- | --- | --- | --- |
0 | 0 | 0 | 0 | 1 | 
1 | 0 | 0 | 1 | 0 | 
2 | 0 | 0 | 1 | 1 | 
3 | 0 | 1 | 0 | 0 | 
4 | 0 | 1 | 0 | 1 | 
5 | 0 | 1 | 1 | 0 | 
6 | 0 | 1 | 1 | 1 | 
7 | 1 | 0 | 0 | 0 | 
8 | 1 | 0 | 0 | 1 | 
9 | 1 | 0 | 1 | 0 | 
10 | 1 | 0 | 1 | 1 | 
11 | 1 | 1 | 0 | 0 | 
12 | 1 | 1 | 0 | 1 | 
13 | 1 | 1 | 1 | 0 | 
14 | 1 | 1 | 1 | 1 | 


## Experimental Results 

| Env | Base or Adaptor | Return (100 episodes) |    조합  |
|  --- | --- | --- | ---- | 
mpe.simple_spread_v3  |  base | $-47.43 \pm(12.14)$ |  -  | 
mpe.simple_spread_v3  |  adaptor 0 | $-47.22 \pm(12.16)$ |  히든 |
mpe.simple_spread_v3  |  adaptor 1 | $-48.86 \pm(13.00)$ |  전체 액션 확률 | 
mpe.simple_spread_v3  |  adaptor 2 | $\textcolor{green}{-46.83} \pm(11.73)$ |  액션 | 
mpe.simple_spread_v3  |  adaptor 3 | $-47.86 \pm(12.75)$ |  관찰 | 
mpe.simple_spread_v3  |  adaptor 4 | $-48.56 \pm(12.84)$ | - |
mpe.simple_spread_v3  |  adaptor 5 | $-48.33 \pm(12.31)$ | - | 
mpe.simple_spread_v3  |  adaptor 6 | $-48.26 \pm(12.00)$ | - | 
mpe.simple_spread_v3  |  adaptor 7 | $-47.98 \pm(11.93)$ | - |
mpe.simple_spread_v3  |  adaptor 8 | $-47.77 \pm(12.35)$ | - |
mpe.simple_spread_v3  |  adaptor 9 | $-48.28 \pm(13.12)$ | - |
mpe.simple_spread_v3  |  adaptor 10 | $\textcolor{green}{-47.07} \pm(12.06)$ |  (액션+히든+관찰) |
mpe.simple_spread_v3  |  adaptor 11 | $-47.94 \pm(12.80)$ | -  |
mpe.simple_spread_v3  |  adaptor 12 | $-48.19 \pm(13.01)$ | - |
mpe.simple_spread_v3  |  adaptor 13 | $-47.64 \pm(12.42)$ | - |
mpe.simple_spread_v3  |  adaptor 14 | $-47.96 \pm(12.83)$ | - |



## Base Evaluation for 100 episodes 

averaged over three sees

| Env | Hyperparams | Return (100 episodes) |
|  --- | --- | --- | 
mpe.simple_crypto_v3  |  4 1 | $0.15 \pm(0.65)$ | 
mpe.simple_crypto_v3  |  4 2 | $0.67 \pm(1.39)$ | 
mpe.simple_crypto_v3  |  4 3 | $0.52 \pm(0.88)$ | 
mpe.simple_crypto_v3  |  8 1 | $0.94 \pm(1.43)$ | 
mpe.simple_crypto_v3  |  8 2 | $0.51 \pm(0.89)$ | 
mpe.simple_crypto_v3  |  8 3 | $1.07 \pm(1.00)$ | 
mpe.simple_push_v3  |  4 1 | $-0.06 \pm(0.04)$ | 
mpe.simple_push_v3  |  4 2 | $-0.06 \pm(0.05)$ | 
mpe.simple_push_v3  |  4 3 | $-0.06 \pm(0.04)$ | 
mpe.simple_push_v3  |  8 1 | $-0.07 \pm(0.05)$ | 
mpe.simple_push_v3  |  8 2 | $-0.06 \pm(0.04)$ | 
mpe.simple_push_v3  |  8 3 | $-0.07 \pm(0.05)$ | 
mpe.simple_reference_v3  |  4 1 | $-0.73 \pm(0.29)$ | 
mpe.simple_reference_v3  |  4 2 | $-2.20 \pm(1.44)$ | 
mpe.simple_reference_v3  |  4 3 | $-0.72 \pm(0.30)$ | 
mpe.simple_reference_v3  |  8 1 | $-0.72 \pm(0.29)$ | 
mpe.simple_reference_v3  |  8 2 | $-0.70 \pm(0.27)$ | 
mpe.simple_reference_v3  |  8 3 | $-0.70 \pm(0.28)$ | 
mpe.simple_speaker_listener_v4  |  4 1 | $-0.55 \pm(0.42)$ | 
mpe.simple_speaker_listener_v4  |  4 2 | $-0.55 \pm(0.42)$ | 
mpe.simple_speaker_listener_v4  |  4 3 | $-0.56 \pm(0.43)$ | 
mpe.simple_speaker_listener_v4  |  8 1 | $-0.56 \pm(0.42)$ | 
mpe.simple_speaker_listener_v4  |  8 2 | $-0.55 \pm(0.42)$ | 
mpe.simple_speaker_listener_v4  |  8 3 | $-0.55 \pm(0.43)$ | 
mpe.simple_spread_v3  |  4 1 | $-0.84 \pm(0.29)$ | 
mpe.simple_spread_v3  |  4 2 | $-0.99 \pm(0.29)$ | 
mpe.simple_spread_v3  |  4 3 | $-0.82 \pm(0.29)$ | 
mpe.simple_spread_v3  |  8 1 | $-0.83 \pm(0.25)$ | 
mpe.simple_spread_v3  |  8 2 | $-1.01 \pm(0.54)$ | 
mpe.simple_spread_v3  |  8 3 | $-0.85 \pm(0.28)$ | 
mpe.simple_tag_v3  |  4 1 | $1.90 \pm(4.09)$ | 
mpe.simple_tag_v3  |  4 2 | $1.33 \pm(3.59)$ | 
mpe.simple_tag_v3  |  4 3 | $1.43 \pm(3.50)$ | 
mpe.simple_tag_v3  |  8 1 | $1.10 \pm(3.23)$ | 
mpe.simple_tag_v3  |  8 2 | $1.13 \pm(3.17)$ | 
mpe.simple_tag_v3  |  8 3 | $1.47 \pm(3.72)$ | 