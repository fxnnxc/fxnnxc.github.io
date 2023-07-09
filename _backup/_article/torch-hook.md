---
layout: post
title: 'Torch Hook based Model Debugging'
date: 2022-05-04 11:12:00-0400
description: 'Functionality of Torch Hook'
tags: Torch
category: TORCH
subcategory : Hook
---

# Hook 

Python is a modular programming language and every methods are called in procedure. There is no way you can control the `forward` or `backward` path in the already defined subclass of `torch.nn.Module`, such as `vgg16` and `transformer`. If you want to examine the model in the procedure you have to use **hook**. There are two types of hooks 

* Tensor Hook
  * `hook`  :  backward hook for a tensor
* Module Hook  
  * `forward_pre_hook` : before `forward` is called
  * `forward_hook` : after `forward` is called
  * `backward_full_hook` : after `backward` is called

# Code

```python
def backward_hook(gradient):
    # do something 
    pass

import torch 
X = torch.tensor([1,2,3])
X.register_hook(backward_hook)
```



```python
def module_forward_pre_hook(module, input):
    pass  

def module_forward_hook(module, input, output):
    pass 

def module_full_backward_hook(moudle, grad_in, grad_out):
    pass

import torch 
import torch.nn as nn

module = nn.Linear(5,5)
module.register_forward_pre_hook(module_forward_pre_hook)
module.register_forward_hook(module_forward_hook)
module.register_full_backward_hook(module_full_backward_hook)
```