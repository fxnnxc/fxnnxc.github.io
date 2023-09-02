---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-08-30
featured: true
img: https://drive.google.com/uc?export=view&id=12KRTKN9v2aYFbpQYuOfsM_o7rn0QMl8i
title: 'Graph Neural Network'
description: 'Summary of learning graph neural network'
---


## Graph Study 

I started studying graph neural network following lectures Stanford CS224W [[playlist](https://www.youtube.com/playlist?list=PLoROMvodv4rPLKxIpqhjhPgdQy7imNkDn)]. This post is the summary and my thoughts on the materials covered in the lectures. 

---

## 🪴 Lecture 1.1 [[Youtube]](https://www.youtube.com/watch?v=JAB_plj2rbA)

This section explains several graphs based on data types, necessity of graph representations, and covered materials in the lectures.  In a graph, entities (vertices) have relationships to each other. Based on the entity and relation types, there are several graph representations.

### Examples of Graphs based on Data Types
* *Event Graphs*
* *Computer Networks* 
* *Disease Pathways* 
* *Food Webs* 
* *Particle Networks* 
* *Underground Networks*
* *Social Network* : Society is a collection of 7+ billion  individuals 
* Economic Networks
* Communication Network : Electronic devices, phone calls, financial transactions 
* Brain Connections : our thoughts are hidden in the connections between billions of neurons 
* Knowledge Graphs 
* Code Graphs 
* Molecules : atoms are vertices and connections (bonds) are edges 
* 3D Shapes 
* Similarity networks
* Relational structures 

> Network or Graph? <br> Sometimes the distinction between networks and graphs is blurred. 


### Difference between Graph and other representations 

Complex domains have a rich relational structure, which can be represented as a graph. The features of graph are as follows:

* Networks are complex : arbitrary size and complex topological structure 
* No fixed node ordering or reference point 
* Dynamic and multimodal features. 

### What can we do with graph? 

Note that the goal of graph is mostly learning representation, that is, having a good representation of graphical data. As such, the input is `network` and output is several graph components such as `Node labels`, `New links`, `Generated graphs` and `subgraphs`. 
With the progress of deep learning, we can automatically extract informative features. A deep neural network map nodes to d-dimensional embeddings such that similar nodes in the network are embedded close together. 

* Traditional methods : Graphlets, Graph Kernels. 
* Methods for node embeddings : DeepWalk, Node2Vec
* Graph Neural Networks :  GCN, GraphSAGE, GAT, Theory of GNNs
* Knowledge graphs and reasoning : TransE, BetaE
* Deep generative models for graphs 
* Applications to Biomedicine, Science, Industry 



---

##  🪴 Lecture 1.2 [[Youtube]](https://www.youtube.com/watch?v=aBHC6xzx9YI&list=RDCMUCBa5G_ESCn8Yd4vw5U-gIcg&index=2)

Lecture 1.2 is about basic components (node and edge) and its applications.
The level of graph representation can be seen as 

* Node Level 
* Community level (subgraph) 
* Graph-level
* Edge-Level

With such representations possible tasks are 

* **Node classification** : predict a property of a node (Categorize online users / items)
* **Link prediction** : predict whether there are missing links between two nodes 
* **Graph classification** : Categorize different graphs 
* **Clustering** : Detect if nodes from a community (social circle detection)
* **Graph generation** : Drug discovery
* **Graph evolution** : physical simulation 

## Node Level Example : Protein 3D structure

Protein is a sequence of Amino acids. Can we predict 3D structure based solely on its amino acid sequence?
The key idea of Alphafold <d-cite key="jumper2021highly" /> is spatial graph where  
* **Nodes** : Amino acids in a protein sequence
* **Edges** : Proximity between amino acids (residues)

<center>
<figure>
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Protein_folding_figure.png/450px-Protein_folding_figure.png" style='width:70%'>
<figcaption markdown="1">
Figure. Alphafold inference of 3D structure given a sequence of amino acid. <br> Image from Wikipedia [Alphafold](https://en.wikipedia.org/wiki/AlphaFold).
</figcaption>
</figure>
</center>

## Edge Level

Unlike node level, edge level is a task of finding proper edges. For example, when users interact with items, the goal is 
to recommend items users might like (predict links). Task learn node embeddings $z_i$ such that 

$$
d(z_{cake_1}, z_{cake_2} ) < d(z_{cake_1}, z_{sweater} )
$$


#### Example: Drug Side Effects 

Many patients take multiple drugs to treat complex or co-existing diseases: For example, 46% of people ages 70-79 take more than 5 drugs (in US) and many patients take more than 20 drugs to treat heart disease, depression, insomnia, etc. 

**👨‍⚕️Task**: Given a pair of drugs predict adverse side effects 

* Nodes : Drugs & Proteins 
* Edges : Interactions (side effects)
* Query : How likely will Simvastatin and Ciprofloxacin, when taken together break down muscle tissue? 

The edge (side effect) can even help discovering new side effects. 


---


## 🪴 Lecture 1.3 [[Youtube]](https://www.youtube.com/watch?v=P-m1Qv6-8cI&list=PLoROMvodv4rPLKxIpqhjhPgdQy7imNkDn&index=3)

Finally, lecture 1.3 deals with basic notations in graph representations such as nodes, edges, degree, and components. When we use graph representations, we must set proper features in data to make node and edges. The choice of the proper network representation of a given domain/problem determines our ability to use network successfully. 

* Objects : nodes, vertices $N$ 
* Interactions : links, edges $E$ 
* System : network, graph $G(N, E)$


### Undirected vs Directed 


#### Undirected 
A graph with no directional edges. For example, collaborations, friendship on facebook. 
<br> Node degree $k_i$ and average degree : $\bar{k} = \< k \> = \frac{1}{N} \sum_{i=1}^N k_i = \frac{2E}{N}$


#### Directed 
A graph with directional edges. For example, phone calls, following in tweets. 
<br> $\bar{k}^{in} = \bar{k}^{out}= \frac{E}{N}$ : the number of in-degree and out-degree is same. 

#### Bipartite Graph 
A graph with two disjoint sets $U$ and $V$

* Authors-to-Papers (they authored)
* Actors-to-Movies (they appeared in)
* Users-to-Movies (they rated)


#### Folded Networks : 

project two sets in the bipartite graph respectively. For example, authors collaboration networks 

<center>
<figure>
<img src="https://drive.google.com/uc?export=view&id=12KRTKN9v2aYFbpQYuOfsM_o7rn0QMl8i" style='width:70%'>
</figure>
</center>

#### Adjacency Matrix 

$A_{ij}=1$ if there is a link from node $i$ to node $j$ and $A_{ij}=0$ otherwise.

#### Represent graph as a list of edges 

Most real-world networks are sparse  $E << E_{max}$ 
```
G = {(1,2), (2,3), (3,4), (3,2)}
```

#### Adjacency list 
```
G is a form of 
1: [] 
2: [3,4,5]
3: [3,4,6]
```


#### Possible Options 

* Weight (frequency of communicating )
* Ranking (best friend, second friend)
* Type  (friend, relative, co-worker)
* Sign (Friend vs Foe, Trust vs Distrust)

#### Strongly Connected Directed

Strongly connected directed graph  has a path from each node to every other node and vice versa (A-B path and B-A path)
Weakly connected directed graph  : is connected if we disregard the edge directions 

#### Strongly Connected Components (SCC)

Strongly connected components (SCCs) can be identified, but not every node is part of a nontrivial strongly connected component. 

* In-component: nodes that can reach the SCC
* Out-component: nodes that can be reached from the SCC. <br> (starting from SCC node to the component)