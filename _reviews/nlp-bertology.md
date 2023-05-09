---
layout: post
title: 'BERTology'
date: 2023-05-09 00:00:00-0400
description: ''
category: NLP
subcategory : Attention
---

## List of Papers

[1] Kovaleva, Olga, et al. "Revealing the Dark Secrets of BERT." Proceedings of the 2019 Conference on Empirical Methods in Natural Language Processing and the 9th International Joint Conference on Natural Language Processing (EMNLP-IJCNLP). 2019.


[2] Rogers, Anna, Olga Kovaleva, and Anna Rumshisky. "A primer in BERTology: What we know about how BERT works." Transactions of the Association for Computational Linguistics 8 (2021): 842-866.


[3] Clark, Kevin, et al. "What Does BERT Look at? An Analysis of BERT’s Attention." Proceedings of the 2019 ACL Workshop BlackboxNLP: Analyzing and Interpreting Neural Networks for NLP. 2019.


[4] Voita, Elena, et al. "Analyzing Multi-Head Self-Attention: Specialized Heads Do the Heavy Lifting, the Rest Can Be Pruned." Proceedings of the 57th Annual Meeting of the Association for Computational Linguistics. 2019.

## Datasets 

* GLUE Benchmark [1] : 
* Sentiments (SST, IMDB, ) : 
* FrameNet [1] : Frame elements correspond to semantic roles for a given frame, for example, “buyer”, “seller”, and “goods” for the “Commercial transaction” frame evoked by the words “sell” and “spend” or “topic” and “text” for the “Scrutiny” semantic frame evoked by the verb “address”.
* WSJ Penn Treebank dependency parsing [3] : word relationship 

## Objectives 

* 

## Metrics 

* 


---

# Revealing the Dark Secrets of BERT [1]

<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px"> Attention Patterns </tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">2019_Kovaleva</tag>

## Attention Patterns 

* Vertical : Attention scores to a specific token such as [SEP] or [CLS]
* Diagonal :Mostly focusing on the token itself. 
* Vertical + Diagonal 
* Block : when two sentences are encoded with seperation token and attentions are constructed within a sentence each. 
* Heterogeneous : some semantic meanings. 


## Measuring Attention Weights

* CLS / SEP, NOUN, OBJ, VERB,... the results show task-specific attention weights. 


## Pruning 
* let average attention score for a specific head. 
* one head at a time : task specific layer. Most heads could be pruned, but there are some heads which give lower performance. 
* one layer at a time : task specific layer 

---

# A Primer in BERTology: What We Know About How BERT Works [2] 

<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px"> Attention Patterns </tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">2019_Rogers</tag>


---

# What does BERT look at? An Analysis of BERT’s Attention [3]

<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">  Quantitative Head Analysis </tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">2019_Clark</tag>


### Quantifying Average Attention and Norm of Gradients 

* Attention 
  * [CLS] : early layer 
  * [SEP] : middle layer
  * [Comma, Dot] :  Last Layer
* Gradients : reduced as the layers become deeper 
* Etropy
  * early: high
  * middle: low 
  * last : high 

### Dependency Parsing 

The best performing attentions heads of BERT on WSJ dependency parsing by dependency type. Wall Street Journal portion of the Penn Treebank (Marcus et al., 1993) annotated with Stanford Dependencies.

* det : 8-11 / ACC: 94.3
* prep : head 7-4 / ACC: 66.7
* nsubj : head 8-2 / ACC: 58.5


### Attenion-Only Probe 


$$
p(i\vert j) \propto \exp \Big(  \sum_{k=1}^n w_k \alpha_{ij}^k + u_k \alpha_{ji}^k \Big)
$$
where $\alpha_{ij}^k$ is the attention weight from word $i$ to word $j$ produced by head $k$, and $n$ is the number of attenion heads. 

### Attention-and-Words Probe 

Linearly combine GloVe embedding with attention score. 


$$
p(i\vert j) \propto \exp \Big(  \sum_{k=1}^n W_{k:} (v_i \oplus v_j ) \alpha_{ij}^k + U_{k:} (v_i \oplus v_j ) \alpha_{ji}^k \Big)
$$


A simple model taking BERT attention maps and GloVe word embeddings as input performs quite well at dependency parsing.

* Attn : 61 UAS 
* Attn + GloVe : 77 UAS

### Clustering Attention Heads 

Distnace between two heads $H_i$ and $H_j$ as:

$$
\sum_{\mathrm{token}\in \mathrm{data}} JS(H_i(\mathrm{token}),H_j(\mathrm{token}) )
$$


Heads within the same layer are often fairly close to each other, meaning that heads within the layer have similar attention distributions.


---
<tag class="box-demo-link" style="background:#b4ffff; color:#000000; font-size:18px">  </tag>
<tag class="box-demo-link" style="background:#64DE3A; color:#000000; font-size:18px">2019_Voita</tag>

# Analyzing Multi-Head Self-Attention: Specialized Heads Do the Heavy Lifting, the Rest Can Be Pruned