
---
layout: post
title: 'Speech Correction'
date: 2023-04-018 00:00:00-0400
description: ''
category: Speech
subcategory : Correction
---


## Speech Correction Task 

Portion of corrected parts are masked 

Prononciation requires target speech 


------

SpeechPainter :Text conditioned speech inpainting 

Speech is corrected 


------


Speech Audio Corrector

* VQVAE codebook is used to encode a waveform. 
* Generate Mel-Spectrogram 


-----

Pruning 으로 모델을 더욱 정확하게 만들 수 있지 않은가?
Why internal unit을 다뤄야 하는가? 

Imperfect model 

**Refinement Phase** 
* Pruning
* Distillation
* Internal Unit Correction : Correct Samples

Providing Nice Framework of the task must be defined. 
