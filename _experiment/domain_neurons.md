---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2023-08-21
featured: true
img: 
title: '[Experiment Note] Domain Neurons'
description: 'Series of experiments conducted for domain neurons'
tags: 
---

### 2023.08.22 

* 📌 Why CartPole checkpoints have the same return (500) for all models? : To validate  the code, we run additional training: Mountain-Car. [[mail-link](https://mail.google.com/mail/u/1/#inbox/FMfcgzGtwgnSXGfkFBPdSMplGBwhZgjq)]
* ✔️ Mountain-Car Training is implemented [[mail-link](https://mail.google.com/mail/u/1/#inbox/FMfcgzGtwgnSXHntGzqXnKwFPgfFrQKh)]
* ✔️ Moutain-Car evaluation showed the increasing return unlike `CartPole`. [[mail-link](https://mail.google.com/mail/u/1/#inbox/FMfcgzGtwgnSXMKgVSSHsBKZrhnzFsNq)]. Although, training is underfit, we don't play with Mountain-Car and CartPole as they are not be used in the Paper.  
* ❌ Initial PPO Training in `CarRacing`.  (failed. The return did not increase.)  [mail-link](https://mail.google.com/mail/u/1/#inbox/FMfcgzGtwgpbdtBcmzrXGtSCZKzTCplg)
  * **This experiment is failed as the return did not increase.** 
* ✔️ CartPole-Randomized environmet  training (V0 ~ V2). (to be done). 
[[mail-link v2](https://mail.google.com/mail/u/1/#inbox/FMfcgzGtwgpbTXmMFNRqqPxGwDkjKNnJ)]
[[mail-link v1](https://mail.google.com/mail/u/1/#inbox/FMfcgzGtwgpbLNbmRMPlKDwrCXhmNHJJ)] 
[[mail-link v0](https://mail.google.com/mail/u/1/#inbox/FMfcgzGtwgpbLHtmGBdFtPTgbhNkLmhN)]
  * **V2 has higher return than v1 due to small pole length has higher return.** 

### 2023.08.23 

* SAC trainer implementation and train on car racing. 
* Domain Randomization 


