---
layout: page
title: '🎯 #3 Four Samples for the dynamics of LRP'
description: a project with a background image
img: assets/img/12.jpg
importance: 1
category: work
---



## Motivation / Overview / Goal

In the previous experiment, I found that there is a proper LRP rule for the explanation of a model. It is known  that LRP rules have specific role to deliver information from back to front. The `gamma rule` prefers positive weights and makes the propagation more highlightly to important parts. the `epsilon rule` removes information by adding constant to a normalization term. `z_plus` rule propagates only positive scores. In the previous experiment, it is prefered to use `z_plus` rule to describe the dynamics of training. However, it is not trivial how can we advance the result further. Therefore, in this experiment, we trick the result of LRP rule to represents the dynamics of training in better way. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/dynamics_of_lrp/measure_dynamics.png" class="img-fluid rounded z-depth-10" zoomable=true %}
    </div>
</div>

<div class="caption">
    LRP scores are calculated while training. The dynamics of LRP score is calculated to represent the pixel wise change. 
</div>


<br>

## Method

This experiment is done based on the result of the previous work. We measure the difference between two successive LRP scores and verify whether there some pixels whose change of the LRP score is significant than other pixels. 

For a given pixel position $$p$$, we compute the global dynamics by the sum of differences between two successive LRP scores $$R_i $$ and $$R_{i+1}$$. 

$$
\text{Dynamics}(p) = \sum_{i=1}^{N-1} |R_i^{(p)} - R_{i+1}^{(p)}| 
$$






<div class="row">
    <div class="col-sm mt-0 mt-md-0">
        {% include figure.html path="assets/img/dynamics_of_lrp/OriginalInput.png" class="img-fluid rounded z-depth-10"  %}
    </div>
</div>

<div class="caption">
    Input image of RL training and the dynamics of LRP heatmap on the image. 
</div>

<br/>

<br/>

### Individual Differences 




<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/dynamics_of_lrp/abs_relevance_difference_3.png" class="img-fluid rounded z-depth-10" zoomable=false %}
    </div>
</div>


When we aggregate all the differences, we get the divergence of the input observation. 





<br/>
### Something wrong..

I found that the distribution of differences is not smooth and there are outlier samples where the value is really high. Therefore, I should try other observation samples and increase the training size. 


<div class="row justify-content-sm-center">
<div class="col-sm-15 mt-md-0">
    {% include figure.html path="assets/img/dynamics_of_lrp/dynamic_measure_plot.png" class="img-fluid rounded z-depth-1"  %}
</div>
</div>