---
layout: post
title: 'Laplace Matrix (beginner)'
date: 2022-04-29 11:12:00-0400
description: None
tags: LinearAlgebra 
category: ML
subcategory : Laplace

---

# Construct Graph

Consider a graph

<center>
<div class="row-sm mt-3">
    <div class="col-sm-5 mt-3 mt-md-0">
        {% include figure.html path="assets/img/knowledge2/data_distribution.png" class="img-fluid rounded z-depth-1" zoomable=true %}
        <p style="color:blue"> Figure 1. Data Distribution </p>
    </div>
</div>
</center>


# Laplace Matrix

<center>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/knowledge2/graph.png" class="img-fluid rounded z-depth-1" zoomable=true %}
        <p style="color:blue"> Figure 2. Graph formulation  </p> 
    </div>
        <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/knowledge2/laplace_matrix.png" class="img-fluid rounded z-depth-1" zoomable=true %}
        <p style="color:blue"> Figure 3. Laplace matrix </p>
    </div>
</div>
</center>

# Eigenvalue Decomposition 

<center>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/knowledge2/eigen_values.png" class="img-fluid rounded z-depth-1" zoomable=true %}
        <p style="color:blue"> Figure 4. Eigenvalues </p>
    </div>
        <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/knowledge2/formula.png" class="img-fluid rounded z-depth-1" zoomable=true %}
        <p style="color:blue"> Figure 5. Eigen decomposition</p>
    </div>
</div>
</center>


<center>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">r
        {% include figure.html path="assets/img/knowledge2/eigen_vectors.png" class="img-fluid rounded z-depth-1" zoomable=true %}
        <p style="color:blue"> Figure 5. Eigen vectors. From left to right, eigen vectors with small eigen values</p>
    </div>
</div>
</center>


# Empirical Clustering 


## 1. Fixed Clusters with Varying Eigevnvectors

<center>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/knowledge2/fixed_cluster_with_varying_eigenvectors.png" class="img-fluid rounded z-depth-1" zoomable=true %}
        <p style="color:blue"> Figure 4. Input gradient example. Because we have no clue about the magnitude of the gradients, its support set must be $(-\infty, \infty)$</p>
    </div>

</div>
</center>

## 2. Fixed Clusters with Single Eigevnvector


<center>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/knowledge2/fixed_cluster_with_single_eigenvectors.png" class="img-fluid rounded z-depth-1" zoomable=true %}
        <p style="color:blue"> Figure 4. Input gradient example. Because we have no clue about the magnitude of the gradients, its support set must be $(-\infty, \infty)$</p>
    </div>

</div>
</center>


## 3. Varying Clusters with Fixed Eigevnvector


<center>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/knowledge2/varying_cluster_with_fixed_eigenvectors.png" class="img-fluid rounded z-depth-1" zoomable=true %}
        <p style="color:blue"> Figure 4. Input gradient example. Because we have no clue about the magnitude of the gradients, its support set must be $(-\infty, \infty)$</p>
    </div>

</div>
</center>

## 3. Varying Clusters with Varying Eigevnvector


<center>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/knowledge2/varying_cluster_with_varying_eigenvectors.png" class="img-fluid rounded z-depth-1" zoomable=true %}
        <p style="color:blue"> Figure 4. Input gradient example. Because we have no clue about the magnitude of the gradients, its support set must be $(-\infty, \infty)$</p>
    </div>

</div>
</center>