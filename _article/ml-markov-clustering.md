---
layout: post
title: 'Markov Clustering'
date: 2022-05-08 00:00:00-0400
description: 'Expansion and Inflation'
tags: Stats, Cluster
category: ML
subcategory : Clustering

---


# 1. Abstract 
<!-- 
많은 양의 데이터가 있을 때, 이를 해석하는 좋은 방법은 유사한 데이터들을 그룹핑하여 만들어진 클러스터들을 해석하는 것이다. 
클러스터를 만드는 기준은 여러 가지가 있는데, 그 중에서 Markov Decision Process에서 영감을 받은 Markov Cluster를 소개한다. 
데이터 포인트들을 Markov적으로 생각하는 방법은 다음과 같다.
먼저 데이터 점들을 노트라고 가정하고, 점들의 길이를 Edge라고 하자. 
그러면 모든 노드들이 연결된 Complete Graph 가 만들어지고, 이 그래프에서 엣지들을 없애는 방향으로 
클러스터를 만들 수 있다. 
하나의 노드에서 출발하여 주변을 돌아다녔을 때 방문하는 점들이 클러스터의 원소가 되는 것이다. 

Markov Decision Process에서 Stationary 한 경우, 하나의 State에서 대한 방문 확률은 이웃들의 확률로부터 안정적으로 구해진다.
한 가지 문제점은 확률값이 대부분 0이 아니고 작은 값을 가지고 있다는 것이다. 이 경우, 멀리 있는 노드에 대한 Edge를 줄여야 한다. 
Markov Cluster 에서는 반복적으로 엣지의 연결성을 Sparse 하게 만듬으로써 클러스터를 강제한다. 
 -->

When large number of samples are distributed, one of the best ways to understand the structure is clustering. 
Several cluster methods have been proposed and Markov Clustering is one method motivated by Markov Decision Process (MDP). 
The perspectives of MDP in clustering is as follows. 

* 🍀 Consider the data points as **states** and edge length as **the next state transition probability**.
* 🍀 The full data could be understood as a **complete graph** and clusters could be formed by **removing edges** so that disjoint subgraphs. 
* 🍀 Edge lengths become **sparse** by the power of the transition matrix. 

In MDP, state transition could be non-zero for other states. As we want to form a clusters, we should make the transitions as sparse. 
Markov Clustering handles this issue by `Inflation` (element ) so that low signals become zero and high signals become higher. 


# 2. Motivation 
<!-- 
클러스터의 샘들은 Markov Decision Process의 State들이고 이웃들은 Transition(Random Walk)으로부터 방문하는 점들이다. Matrix에 대한 지수승으로 연결성을 Sparse 하게 만들면서 그래프의 엣지를 없앤다. 
 -->
 The elements in a cluster is states in Markov Decision Process and neighbors are possible next states. In Markov Cluster, edge connects become sparse by the power of matrix. 
The constructed clusters are formed by the edge distances. 

# 3. Method

Assume that we have a complete graph $G$ whose edge is the distance between two vertices. 
The markov clustering is an iterative algorithm which propagates signals to neighborhoods and makes signals sparse.  
There are two operations `Expansion`  and `Inflation`. 

1. 🚀 **Expansion** : propagate signals to neighbors with edges.  
2. 🚀 **Inflation** : make signals sparse by making large signal larger and small signal smaller. 



<hr/>
<center>
<h2 style='border:5px outset; color:blue; text-align:center; width:200px; padding:2px;'>
Expansion 
</h2>
</center>



Let $M_0$ be a $n\times n$ matrix whose element is the distance to another node at time $t$. The `expansion` operation propagate signals to neighborhoods with matrix multiplication.   

$$
M_{t+1} \leftarrow (M_{t})^p
$$

Here $p$ is a hyperparameter which controls the transition length of the signal. That is, if $p=2$, only direct neighborhoods are communicated.  

<hr/>

<center>
<h2 style='border:5px outset; color:orange; text-align:center; width:200px; padding:2px;'>
Inflation
</h2>
</center>


Given a matrix $M \in \mathbb{R}^{k\times l}, M \ge 0$ and a real nonnegative number $r$. 
Inflation operation is defined by 

$$
M_{t+1, ab} = (M_{t, ab})^r / \sum_{k=1}^k (M_{t, kb})^r
$$

where $a,b$ is the edge distance from $a$ to $b$ and $t$ is the iteration number. 



<center>
<div class="row mt-3">
        <div class="col-sm mt-3 mt-md-0">
            {% include figure.html path="assets/img/exp14/animation_cluster.gif" class="img-fluid rounded z-depth-1" zoomable= true %}
        </div>
</div>
</center>


#### Code Implementation 


```python
def expansion(matrix, p):
    assert p>=1
    from numpy.linalg import matrix_power
    return matrix_power(matrix, p)  # information communication. 

def inflation(matrix, r):
    assert r > 0 
    inflated_matrix = matrix**r
    normalized = normalize_column(inflated_matrix)
    return normalized

def normalize_column(matrix):
    assert matrix.ndim == 2
    normalized = matrix / matrix.sum(axis=0)
    return normalized
```






# 4. Discussions 

Strengths

* If we can view the data points as random walkable, Markov Clustering is a suitable way of cluster them 
* Easy intuition and implementation 
* The order $p$ is based on information communication  



Weaknesses

* Matrix multiplication requires $\mathcal{O}(N^2)$ complexity. 
* Even though iteration makes edges sparse, we need threshold to cut them.
* The order $p$ is a hyperparameter and we should tune them. 

# 5. Related Work 


* Unlike Markov Transition, <tag class="text-box" style='color:red;'>Spectral Clustering</tag>   is based on removing edges which have little affect on the graph structure. 
*  <div><tag class="text-box" style='color:red;'>KNN</tag> or <tag class="text-box" style='color:red;'>K-means</tag>  also consider the neighborhoods, but Markov Clustering assumes graph structure. </div>



#  References 

* [Clustering on Graphs: The Markov Cluster Algorithm (MCL)](https://sites.cs.ucsb.edu/~xyan/classes/CS595D-2009winter/MCL_Presentation2.pdf)
* [Markov Clustering Demo](https://micans.org/mcl/ani/mcl-animation.html)
* [MCL -  a cluster algorithm for graphs](https://micans.org/mcl/)
* Protein sequences clustering of herpes virus by using Tribe Markov clustering (Tribe-MCL)
* [Performance Criteria for Graph Clustering and Markov Cluster Experiments]()



<hr/>


## Full Markov Clustering Code 

```python 
def markov_clustering(matrix, expansion_p, inflation_r, max_iteration=300, save_progress=False):
    iteration = 0 
    matrix = normalize_column(matrix)  # Transition 
    progress = []

    while True:
        if iteration >=max_iteration:
            break 
        matrix_1 = expansion(matrix, expansion_p)  # step 1 expansion (information communication)
        matrix_2 = inflation(matrix_1,inflation_r) # step 2 inflation (sparse)
        matrix = matrix_2
        iteration += 1        

    indices = matrix.argmax(axis=0)
    output = {
        "indices" : indices, 
        "clusters" : get_cluster_index(matrix),
        "matrix" : matrix,
        "iteration" : iteration,
        "progress" : progress
    }
    return output
```


