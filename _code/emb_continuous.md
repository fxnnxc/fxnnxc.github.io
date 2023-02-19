
Code from Shuang Li
https://github.com/ShuangLI59/ebm-continual-learning


## Structure 

* dataloader 
* network
* continual_learner
* callbacks
* main
* param_stamp
* param_values
* train
* utils
    * Loss function  (Knowledge Distillation)
    * Loss function  (Binary Knowledge Disillation)
    * making a datalaoder with `num_workers`
    * to_one_hot : given label and classes, return torch one-hot vector

* visual_plt : visualization plot is defined for analysis 
    * additionally got torchvision grid and imshow mechanism 


```python
# onehot
def to_one_hot(y, classes):
    """
    y: list of labels 
    classes: number of classes
    """
    assert max(y) < classes
    one_hot = torch.zeros(len(y), classes)
    one_hot[range(len(y)),y]= 10.0
    return one_hot
```


```python
# plot 
import torch 
import torchvision
import numpy as np 
import matplotlib.pyplot as plt 
from torchvision.utils import make_grid

tensors = torch.rand(24, 3, 32,32)
image_grid  = make_grid(tensors, nrow=8, pad_value=1)
fig = plt.figure(figsize=(8,3))
plt.imshow(np.transpose(image_grid.numpy(), (1,2,0)))
plt.xticks([])
plt.yticks([])
plt.show()

```

