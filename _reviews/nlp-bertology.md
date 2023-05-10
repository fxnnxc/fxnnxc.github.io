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


## The list of Topics in BERTology 

### Knowledge

* <strong style='color:blue'> BERT ‘‘naturally’’ learns some syntactic information, although it is not very similar to linguistic annotated resources. </strong>
  * BERT representa tions are hierarchical rather than linear, that is, there is something akin to syntactic tree structure in addition to the word order information.
  * syntactic structure is not directly encoded in self-attention weights. (full syntactic subtress could not be recovered with the self-attention weights)
  * syntactic information can be recovered from BERT token representations.
* <strong style='color:blue'>  Either BERT’s syntactic knowledge is incomplete, or it does not need to rely on it for solving its tasks. </strong>
  * BERT does not understand negation and is insensitive to malformed input. In particular, its predictions were not altered even with shuffled word order, truncated sentences, removed subjects and objects (Ettinger, 2019)
* <strong style='color:blue'> Semantic Knowledge  </strong>
  * BERT encodes information about entity types, relations, semantic roles, and proto-roles, since this information can be detected with probing classifiers. (Tenney, 2019b)
  * No evidence from an MLM probing study that BERT has some knowledge of semantic roles (Ettinger, 2019)
  * BERT struggles with representations of numbers.
  * The model does not actually form a generic idea of named entities, although its F1 scores on NER probing tasks are high (Tenney et al., 2019a).
* <strong style='color:blue'> World Knowledge </strong>
  * BERT struggles with pragmatic inference and role-based event knowledge (Ettinger, 2019). BERT also struggles with abstract attributes of objects, as well as visual and perceptual properties that are likely to be assumed rather than mentioned (Da and Kasai, 2019).
  * for some relation types, vanilla BERT is cometitive withmethods relying on knowledge bases
  * BERT cannot reason based on its world knowledge. Forbes et al. (2019) show that BERT can ‘‘guess’’ the affordances and properties of many objects, but cannot reason about the relationship between properties and affordances.
  * For example, it ‘‘knows’’ that people can walk into houses, and that houses are big, but it cannot infer that houses are bigger than people.
* <strong style='color:blue'>  </strong>


### Embeddings

* Input Embeddings 
* Output Embeddings (BERT Embeddings)

* Several studies reported that distilled contextualized embeddings better encode lexical semantic information (i.e., they are better at traditional word-level tasks such as word similarity). 
* The methods to distill a contextualized representation into static include aggregating the information across multiple contexts (Akbik et al., 2019; Bommasani et al., 2020)
* BERT embeddings occupy a narrow cone in the vector space, and this effect increases from the earlier to later layers. 
* That is, two randomwords will on average have amuch higher cosine similarity than expected if em- beddings were directionally uniform (isotropic). Because isotropy was shown to be beneficial for static word e beddings (Mu and Viswanath, 2018)
* the representations of the same word depend on the position of the sentence in which it occurs,

### Heads

* The "heterogeneous" attention pattern could potentially be linguistically interpretable
* some BERT heads seem to specialize in certain types of syntactic relations. Htut et al. (2019) and Clark et al. (2019) report that there are BERT heads that attended significantly more than a random baseline to words in certain syntactic positions.
* no single head has the complete syntactic tree information,
* attention weights are weak indicators of subject-verb agreement and reflexive anaphora. (the interpretation of a representation is differ depending on contexts. )
* most self-attention heads do not directly encode any non-trivial linguistic information, since only fewer than 50% of heads exhibit the ‘‘heterogeneous’’ pattern.

### Layers

* the lower layers have the most information about linear word order.
* syntactic information is most prominent in the middle layers of BERT.
* The final layers of BERT are the most task-specific. (middle layer is transferable (generalizable to any task) and freezing lower layer does not hurt performance. )
* semantics is spread across the entire model

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