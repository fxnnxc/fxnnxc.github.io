---
layout: post
title: 'Knowledge Distillation of ResNet18'
date: 2024-04-18 00:00:00
description: Learn a separate classifier given hooked forward representation. 
tags: vision
categories: vision
---



# Knowledge Distillation 


```python
# Define Loss function as KLDiv
loss_fn = nn.KLDivLoss(log_target=True, reduction='batchmean')

# Get Hooked forwarding signals
target = teacher_model.model(x).detach()                
features = teacher_model.get_hooked_result()

# Predict and compute the loss
prediction = student_model(features)
loss = loss_fn(F.log_softmax(prediction, dim=-1), F.log_softmax(target, dim=-1))
```


## Hooks 

```python

```

## Modeling Separate Classifiers

```python
class Model1(nn.Module):
    def __init__(self):
        super().__init__()
        self.classifier = nn.Linear(512, 1000, bias=True)
        nn.init.kaiming_uniform_(self.classifier.weight)
        
    def forward(self, xs):
        x = self.classifier(xs[-1])
        return x 

class Model2(nn.Module):
    def __init__(self):
        super().__init__()
        self.projection = nn.Linear(512, 1000, bias=True)
        self.classifier = nn.Linear(512, 1000, bias=True)
        nn.init.kaiming_uniform_(self.projection.weight)
        nn.init.kaiming_uniform_(self.classifier.weight)
        
    def forward(self, xs):
        x1 = self.projection(xs[-2])
        x2 = self.classifier(xs[-1])
        x = x1 + x2 
        return x 
```



# Experiment 



# Results 

## Training Loss $(\downarrow)$ 

|---------|----------|----------|----------|----------|----------|
|Models   |  Epoch 1 |   Epoch2 |   Epoch3 |  Epoch 5 | Epoch 10 |
| `v1`    |  
| `v2`    |  
| `v3`    | 
| `v-full`| 


## Validation Accuracy $(\uparrow)$

|---------|----------|----------|----------|----------|----------|
|Models   |  Epoch 1 |   Epoch2 |   Epoch3 |  Epoch 5 | Epoch 10 |
| `v1`    |  
| `v2`    |  
| `v3`    | 
| `v-full`| 



