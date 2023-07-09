---
layout: post
title: '👨🏻‍💻 Knowledge Distillation of ResNet18'
date: 2023-04-19 00:00:00
description: Learn a separate classifier given hooked forward representation. 
tags: vision
categories: vision
---


# Distillation of ResNet18 

The goal of this `rudiment` is re-training `ResNet18` classifier with newly randomized weights. In addition, we extract the representation in `Resnet18` and map to the classifier with `key-value memory` module which consists of Key, Value, and Projection linear layers. We found that if the branch has more weights, it is hard to distill the original `ResNet18`, that is, the simpler model has significantly faster optimization process compared to architectures with more weights. 

## General Results 

* Best performance is obtained with simple models (Model1 and Model2)
* Key-value memory is hardly trained. 
* GELU activation is better than ReLU activation
* Residual connection is worse than densely connected layers. However, the performance difference is not that big. 
* Reducing the training loss is an important task as the evaluation score positively correlates with the training loss. 
* `lr_scheduler` is a good way of distill the model. 


---

# Methods 


There are three key components, 

1. Knowledge Distillation : How to distill the teacher model. 
2. Hooks  : How to get the internal representations of convolutions
3. Models : What would be the student model 



# 1. Knowledge Distillation 

We distill the ResNet18 with KLDivergence loss. 

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


## 2. Hooks 


```python
def fn(module, input, output):
    if output.shape == (512, 8,8):  # the last convolution layer of resnet18
        output_size =1 
    elif output.shape == (256, 16,16): 
        output_size = 2
    elif output.shape == (128, 32,32): 
        output_size = 4
    elif output.shape == (64, 64, 64):
        output_size = 8
    else:
        output_size = 1
    output = F.adaptive_avg_pool2d(output,  output_size=output_size)
    output  = torch.flatten(output, 1)
    module.output = output 
    
return fn
```

## 3. Modeling Separate Classifiers

* <tag class="box-demo-link" style="background:#78EFC4; color:#000000; font-size:18px">Model1</tag> : retrain the last layer (classifier)
* <tag class="box-demo-link" style="background:#78EFC4; color:#000000; font-size:18px">Model2</tag> : retrain the last two layers (classifier + last convolution)
* <tag class="box-demo-link" style="background:#78EFEB; color:#000000; font-size:18px">Model3</tag> : `Key-value Memory` for last 3 layers **with** `required_grad` on memory
* <tag class="box-demo-link" style="background:#78EFEB; color:#000000; font-size:18px">Model4</tag> : `Key-value Memory` for last 3 layers **without** `required_grad` on memory
* <tag class="box-demo-link" style="background:#787AEF; color:#FFFFFF; font-size:18px">Model5</tag> : `Key-value Memory` with `relu activation` for last 3 layers **with** `required_grad` on memory
* <tag class="box-demo-link" style="background:#787AEF; color:#FFFFFF; font-size:18px">Model6</tag> : `Key-value Memory` with `relu activation` for last 3 layers **without** `required_grad` on memory
* <tag class="box-demo-link" style="background:#000000; color:#FFFFFF; font-size:18px">Model7</tag> : `Key-value Memory` with residual connection for last 3 layers **with** `required_grad` on memory



# 4. Experiment 

We train the models 1 to 8 with features obtained from a resnet18 classifier with the following training hyperparameters. 
To utilize the training, additionally test the `lr_scheduler` version of the original training. 

```python
epochs = 10 
batch_size = 64 
lr = 1e-4
weight_decay = 1e-4
num_workers = 16 
lr_gamma = 0.5
lr_step_size = 2
```


# Results 

## Training Loss $(\downarrow)$ 

|Models |  Epoch 1 |   Epoch 2 |   Epoch 3 |  Epoch 5 | Epoch 10 |
|---|----|----|----------|----------|----------|
| lr_<tag class="box-demo-link" style="background:#78EFC4; color:#000000; font-size:18px">Model1</tag> |0.868 | 0.145 | 0.110 | 0.095 | **0.088** | 
| lr_<tag class="box-demo-link" style="background:#78EFC4; color:#000000; font-size:18px">Model2</tag> |0.858 | 0.146 | 0.110 | 0.096 | 0.089 | 
| lr_<tag class="box-demo-link" style="background:#78EFEB; color:#000000; font-size:18px">Model3</tag> |0.680 | 0.358 | 0.325 | 0.313 | 0.309 | 
| lr_<tag class="box-demo-link" style="background:#78EFEB; color:#000000; font-size:18px">Model4</tag> |0.829 | 0.351 | 0.303 | 0.288 | *0.284* | 
| lr_<tag class="box-demo-link" style="background:#787AEF; color:#FFFFFF; font-size:18px">Model5</tag> |0.736 | 0.415 | 0.379 | 0.370 | 0.377 | 
| lr_<tag class="box-demo-link" style="background:#787AEF; color:#FFFFFF; font-size:18px">Model6</tag> |0.924 | 0.409 | 0.351 | 0.332 | 0.326 | 
| lr_<tag class="box-demo-link" style="background:#000000; color:#FFFFFF; font-size:18px">Model7</tag> |0.605 | 0.354 | 0.321 | 0.310 | 0.305 | 
|----------|----------|----------|----------|----------|----------|
| <tag class="box-demo-link" style="background:#78EFC4; color:#000000; font-size:18px">Model1</tag> |0.864 | 0.145 | 0.112 | 0.101 | **0.099** | 
| <tag class="box-demo-link" style="background:#78EFC4; color:#000000; font-size:18px">Model2</tag> |0.863 | 0.146 | 0.113 | 0.102 | 0.101 | 
| <tag class="box-demo-link" style="background:#78EFEB; color:#000000; font-size:18px">Model3</tag> |0.674 | 0.353 | 0.338 | 0.326 | 0.316 | 
| <tag class="box-demo-link" style="background:#78EFEB; color:#000000; font-size:18px">Model4</tag> |0.840 | 0.354 | 0.319 | 0.297 | *0.281* | 
| <tag class="box-demo-link" style="background:#787AEF; color:#FFFFFF; font-size:18px">Model5</tag> |0.736 | 0.416 | 0.398 | 0.384 | 0.368 | 
| <tag class="box-demo-link" style="background:#787AEF; color:#FFFFFF; font-size:18px">Model6</tag> |0.906 | 0.400 | 0.359 | 0.334 | 0.316 | 
| <tag class="box-demo-link" style="background:#000000; color:#FFFFFF; font-size:18px">Model7</tag> |0.605 | 0.352 | 0.341 | 0.330 | 0.320 | 



## Validation Accuracy $(\uparrow)$


|Models   |  Epoch 1 |   Epoch 2 |   Epoch 3 |  Epoch 5 | Epoch 10 |
|---------|----------|----------|----------|----------|----------|
| lr_<tag class="box-demo-link" style="background:#78EFC4; color:#000000; font-size:18px">Model1</tag> |0.683 | 0.692 | 0.695 | 0.695 | **0.696** | 
| lr_<tag class="box-demo-link" style="background:#78EFC4; color:#000000; font-size:18px">Model2</tag> |0.684 | 0.692 | 0.695 | 0.696 | **0.696** | 
| lr_<tag class="box-demo-link" style="background:#78EFEB; color:#000000; font-size:18px">Model3</tag> |0.626 | 0.627 | 0.636 | 0.641 | 0.645 | 
| lr_<tag class="box-demo-link" style="background:#78EFEB; color:#000000; font-size:18px">Model4</tag> |0.631 | 0.640 | 0.649 | 0.653 | *0.656* | 
| lr_<tag class="box-demo-link" style="background:#787AEF; color:#FFFFFF; font-size:18px">Model5</tag> |0.611 | 0.617 | 0.627 | 0.631 | 0.633 | 
| lr_<tag class="box-demo-link" style="background:#787AEF; color:#FFFFFF; font-size:18px">Model6</tag> |0.618 | 0.627 | 0.639 | 0.647 | 0.651 | 
| lr_<tag class="box-demo-link" style="background:#000000; color:#FFFFFF; font-size:18px">Model7</tag> |0.627 | 0.628 | 0.636 | 0.640 | 0.645 | 
|----------|----------|----------|----------|----------|----------|
| <tag class="box-demo-link" style="background:#78EFC4; color:#000000; font-size:18px">Model1</tag> |0.684 | 0.692 | 0.693 | 0.693 | 0.693 | 
| <tag class="box-demo-link" style="background:#78EFC4; color:#000000; font-size:18px">Model2</tag> |0.685 | 0.690 | 0.692 | **0.694**| 0.692 | 
| <tag class="box-demo-link" style="background:#78EFEB; color:#000000; font-size:18px">Model3</tag> |0.629 | 0.628 | 0.633 | 0.631 | 0.636 | 
| <tag class="box-demo-link" style="background:#78EFEB; color:#000000; font-size:18px">Model4</tag> |0.630 | 0.639 | 0.641 | 0.644 | *0.646* | 
| <tag class="box-demo-link" style="background:#787AEF; color:#FFFFFF; font-size:18px">Model5</tag> |0.613 | 0.616 | 0.619 | 0.622 | 0.622 | 
| <tag class="box-demo-link" style="background:#787AEF; color:#FFFFFF; font-size:18px">Model6</tag> |0.621 | 0.632 | 0.638 | 0.638 | 0.642 | 
| <tag class="box-demo-link" style="background:#000000; color:#FFFFFF; font-size:18px">Model7</tag> |0.627 | 0.629 | 0.632 | 0.632 | 0.634 | 

## Coding



```python
class KeyValueMemory(nn.Module):
    def __init__(self, in_features, requires_grad, activation='gelu', hidden_dim=256, out_dim=1000):
        super().__init__()
        self.key = nn.Linear(in_features, hidden_dim, bias=True)
        self.memory = nn.Parameter(torch.zeros(hidden_dim, 512), requires_grad=requires_grad)
        self.projection = nn.Linear(512, out_dim, bias=True)
        if activation == 'gelu':
            self.activation = nn.GELU()
        elif activation == 'relu':
            self.activation = nn.ReLU()
        else:
            assert False             
        nn.init.kaiming_uniform_(self.memory)
        nn.init.kaiming_uniform_(self.key.weight)
        nn.init.kaiming_uniform_(self.projection.weight)
        
    def forward(self, x):
        x = self.key(x) 
        x = self.activation(x)
        x = F.linear(x, self.memory.T)
        x = self.activation(x)
        x = self.projection(x)
        return x 


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

class Model3(nn.Module):
    def __init__(self):
        super().__init__()
        self.kvm1 = KeyValueMemory(256, True)
        self.kvm2 = KeyValueMemory(512, True)
        self.kvm3 = KeyValueMemory(512, True)

    def forward(self, xs):
        x1 = self.kvm1(xs[-3])
        x2 = self.kvm2(xs[-2])
        x3 = self.kvm3(xs[-1])
        x = x1 + x2 + x3
        return x 

class Model4(nn.Module):
    def __init__(self):
        super().__init__()
        self.kvm1 = KeyValueMemory(256, False)
        self.kvm2 = KeyValueMemory(512, False)
        self.kvm3 = KeyValueMemory(512, False)
    
    def forward(self, xs):
        x1 = self.kvm1(xs[-3])
        x2 = self.kvm2(xs[-2])
        x3 = self.kvm3(xs[-1])
        x = x1 + x2 + x3
        return x 


class Model5(nn.Module):
    def __init__(self):
        super().__init__()
        self.kvm1 = KeyValueMemory(256, True, activation='relu')
        self.kvm2 = KeyValueMemory(512, True, activation='relu')
        self.kvm3 = KeyValueMemory(512, True, activation='relu')

    def forward(self, xs):
        x1 = self.kvm1(xs[-3])
        x2 = self.kvm2(xs[-2])
        x3 = self.kvm3(xs[-1])
        x = x1 + x2 + x3
        return x 

class Model6(nn.Module):
    def __init__(self):
        super().__init__()
        self.kvm1 = KeyValueMemory(256, False, activation='relu')
        self.kvm2 = KeyValueMemory(512, False, activation='relu')
        self.kvm3 = KeyValueMemory(512, False, activation='relu')

    def forward(self, xs):
        x1 = self.kvm1(xs[-3])
        x2 = self.kvm2(xs[-2])
        x3 = self.kvm3(xs[-1])
        x = x1 + x2 + x3
        return x 


class Model7(nn.Module):
    def __init__(self):
        super().__init__()
        self.kvm1 = KeyValueMemory(256, True, activation='gelu', hidden_dim=512, out_dim=512)
        self.kvm2 = KeyValueMemory(512, True, activation='gelu', hidden_dim=512, out_dim=512)
        self.kvm3 = KeyValueMemory(512, True, activation='gelu', hidden_dim=512, out_dim=1000)
    
    def forward(self, xs):
        x1 = self.kvm1(xs[-3])
        x2 = self.kvm2(xs[-2] + x1)
        x3 = self.kvm3(xs[-1] + x2)
        x = x3
        return x 
```
