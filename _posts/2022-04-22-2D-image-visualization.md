---
layout: post
title: '🎯 #5 Image Visualization'
date: 2022-04-28 11:53:01
description: an example of a blog post with some math
tags: Vision, Visualization
categories: Vision
---


# 1. Motivaiton of this expeirment

High dimensional vector is complex, while an image of the vector is easily understood. Visualization of the vector is essential to get insight. However, it is hard to properly visualize the vector because there are multiple channels and our view point is 2D. Therefore, proper visualization technique must be discussed. 


<br/>
<h1 style="font-family:Cursive">  2. Setup 🎯 </h1>

<h3 > 2.1 Image as Math </h3> 

First consider an image $I \in V^{W\times H \times C}$ and pixel $I_p^c \in V$ at position $p \in (W) \times (H)$ in channel $C$ where $(W) = \set{0,1,\cdots, W-1}$. A grey image can be viewed as image $I$ with number of channel $C=1$. In the case of RGB image, $V = [0,1]$ or $V=\set{0,1,2,\cdots 255}$. 


<h3 > 2.2 Multi Channel to Grey </h3>

Now consider, we have Image $I$ with $C>1$. The grey scale transformation $G$ can be computed as follows. 

$$\Large{G(I)_p = \frac{1}{C} \sum_{c=0}^{C-1} I_p^c }$$


<h3 > 2.3 Value Interval of Image </h3>

In this formulation, the value interval $V$ must be bounded to describe the maximum and minimum value. It is assumed that all the channels have the same interval $V$. Now consider the minimum and the maximum value of $V$.  For a given color bar, the color must be defined properly only for the values in $V$ as described in Figure 1.

<center>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/exp5/colorbar.png" class="img-fluid rounded z-depth-1" zoomable=false %}
        <p style="color:blue"> Figure 1. The minimum and maximum value in $V$. </p>
    </div>
</div>
</center>


#### Value Interval Example 

Consider the case where pixel values are uniformly distributed in $[0,1]$. Assume there are two Value intervals $V_1 = [0,1]$  which is covered by all the pixels in the image and $V_2 = [0,2]$ which can not be covered by the pixels. You can easily impelment it with python code. 


```python
import numpy as np 
import matplotlib.pyplot as plt
from mpl_toolkits.axes_grid1 import make_axes_locatable

X = np.random.random(size=(32,32,1))
V = [[0, 1], [0, 2]]

fig, axes = plt.subplots(1,2)
for i, ax in enumerate(axes):
    ax = axes[i]
    v_min, v_max = V[i]
    divider = make_axes_locatable(ax)
    cax = divider.append_axes('right', size='5%', pad=0.05)
    im = ax.imshow(X, vmin=v_min, vmax=v_max, cmap="winter")
    fig.colorbar(im, cax=cax, orientation='vertical')
    ax.set_title(f"V_min : {v_min} V_max : {v_max}")

plt.tight_layout()
```

Two images with different value intervals are described in the Figure 2. You can see that we may intepret the image of $V_1$ as randomly distributed while $V_2$ has lower variance, even though there is no difference between two values. This phenomenon motivates more concrete and proper range of the values. 

<center>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/exp5/v_min_v_max.png" class="img-fluid rounded z-depth-1" zoomable=false %}
        <p style="color:blue"> Figure 2. The effect of Value Interval $V$ </p>
    </div>
</div>
</center>




<h1 style="font-family:Cursive">  3. Two kinds of heatmaps 🎯 </h1>


As we discussed in the example, the value interval highly affects the interpretation. Therefore, it is recommanded to pre-define the value interval $V$ before you make an image from numbers. We can consider two types of values, *real world value* and *relative value*. 

<h3 > 3.1 Real World Value </h3>

In the case of `real worl value`, the value itself represents the degree of RGB light, or the brightness in grey image. Therefore $V = [0,1]$ must be guranteed. If your image is real world. Make sure that $V_{max} = 1$ and $V_{min} = 0$. Note that it means you can't normalize the interval like $V_{max} = max_p (I_p^c)$.


<h3 > 3.2 Relative Value </h3>

There are many images which is not real world image, such as 2D heatmap of tempature as in Figure 3. To plot the *relative value*, you must pre-define it so that there is no value outside the interval. If you plot just one instance, setting $V_{max} = \max({I_p^c)}$ is fine. However, when you plot multiple images, you should not use $V_{max} = \max{(I_p^c (x))}$ for each $x$, because in most cases, it will lead to result in Figure 2.

<center>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/exp5/real_vs_relative.png" class="img-fluid rounded z-depth-1" zoomable=false %}
        <p style="color:blue"> Figure 3. Example of real world image and relativevalue image. the real pixel value can be approximated by the image, on the other hand, the value of right image can not be approximated </p>
    </div>
</div>
</center>


<h1 style="font-family:Cursive">  4. Input Attribution 🎯 </h1>


In many machine learning experiments, people measure the input gradient to measure the importance of the pixels in prediction tasks. They use a *real world images* such as dog, cat, and horse. However, the input attribution is *relative value*, meaning, the value range must be defined properly to visual effects. 

For prediction Model $F : \text{Images} \rightarrow \mathbb{R}$, the input gradient of pixel $p$ in channel $c$ is calculated by 

$$
Attr_p^c = \Large{\frac{\partial F}{\partial I_p^c}}
$$

Figure 4 shows the overview of an input attribution. In the case of deep learning, the function is non-linear and not convex function with respect to the input. In addition, we can not analytically find the overall shape of the function. 

<center>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/exp5/input_gradient.png" class="img-fluid rounded z-depth-1" zoomable=false %}
        <p style="color:blue"> Figure 4. Input gradient example. Because we have no clue about the magnitude of the gradients, its support set must be $(-\infty, \infty)$</p>
    </div>
</div>
</center>


In this case, the value interval $V = (\-\infty, \infty)$.  As we discuessed earlier, we can not visualize the infinite range of the values. If we assume the distribution of $\nabla F$ such as Gaussian, we get a value interval by clipping the overall values by mean and variance like

$$
\begin{aligned}
\large{V_{max} =  \mu(Attr) + 3 * \sigma(Attr)} \\
\large{V_{min} =  \mu(Attr) - 3 * \sigma(Attr) }
\end{aligned}
$$

where $\mu(Attr) = \frac{1}{W\times H \times C} \sum_c \sum_p  Attr_p^c$ is the mean of the target values and the $\sigma(Attr)$ is the standard deviation of the values. **Note that this normalization and clipping may lose information. If you think the outliers have meaning in visualization "Do not" clip it.**

<h1 style="font-family:Cursive">  5. Separation of the values 🎯 </h1>

In the previous case, the values are assumed to be continous and the effect of the color is one way. On the other hand, you can consider the case where values are divided by groups for positive and negative values. 
 $V = \set{v | v \in V s.t. v \ge 0} \cup \set{v | v \in V s.t. v<0}$. In this case, you can just select different colors for each group and visualize it as in Figure 5. 


<center>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/exp5/separation.png" class="img-fluid rounded z-depth-1" zoomable=false %}
        <p style="color:blue"> Figure 5. Separation of values by different colors</p>
    </div>
</div>
</center>


You can implement it with python code. 


```python
import numpy as np 
import matplotlib.pyplot as plt
from mpl_toolkits.axes_grid1 import make_axes_locatable
from numpy.ma import masked_array
from matplotlib import colors

Attr = np.array([[-1,-2, -3, -2, -1],
            [-1, 0, 0, 0, -1],
            [-1, 2, 3,  2, -1],
            [-1, 1, 0, 1, -1],
            [-2,-1, -2, -2, -1]])

fig, axes = plt.subplots(1,3, figsize=(8,3))

ax = axes[0]
divider = make_axes_locatable(ax)
cax = divider.append_axes('right', size='5%', pad=0.05)
im = ax.imshow(Attr, vmin=Attr.min(), vmax=Attr.max(), cmap="winter")
fig.colorbar(im, cax=cax, orientation='vertical', shrink=0.25)

ax = axes[1]
divnorm=colors.TwoSlopeNorm(vmin=Attr.min(), vcenter=0., vmax=Attr.max())
divider = make_axes_locatable(ax)
cax = divider.append_axes('right', size='7%', pad="5%")
im = ax.imshow(Attr, cmap="seismic", norm=divnorm )
cb = fig.colorbar(im, cax=cax, orientation='vertical')

ax = axes[2]
divider = make_axes_locatable(ax)
cax = divider.append_axes('top', size='7%', pad="5%")
im = ax.imshow(masked_array(Attr, Attr<0), vmin=0, vmax=Attr.max(), cmap="Reds" )
cb = fig.colorbar(im, cax=cax, orientation='horizontal')
cb.ax.xaxis.set_ticks_position('top')
cb.ax.xaxis.set_label_position('top')
cax = divider.append_axes('right', size='7%', pad="5%")
im = ax.imshow(masked_array(Attr, Attr>0), vmin=Attr.min(), vmax=0, cmap="Blues_r", interpolation='nearest')
fig.colorbar(im, cax=cax, orientation='vertical')

plt.tight_layout()
```