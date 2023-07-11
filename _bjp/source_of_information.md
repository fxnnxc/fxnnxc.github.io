---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-07-10
featured: true
toc:
  - name: Problems
title: 'Source of Information'
description: 'Can we provide the source of generation?'
img: /assets/side_articles/deconvolution/checkboard_example.png
importance: 2 
---

> This is a problem formulation for finding the source of generation. 

## Introduction 


In the training of a deep neural network, we collect a large amount of data and process the data only for training purposes. 
The pre-process mostly decreases the amount of information to abandon irrelevant information in data and provides compact representations for the training data by discarding information which is not helpful for the training. The process of filtering information is essential to properly match the training objective efficiently and robustly <d-footnote> For example, in the classification model, we use correlative features. In the field of NLP, we use natural sentences without meta data. </d-footnote>. Although such filtering can benefit the training, we discard even necessary meta data which helps to verify the information such as **the source of data**. One example would be the copyright of lyrics or contents in a book. For example, can you verify the source of the sentence below? That is, who said the sentence.
> Creating safe AGI that benefits all of humanity  <br>   - From : ???

The source of the sentence is [website of OpenAI](https://openai.com/). As the source is entangled with the sentence, we get additional information such as
* The underlying context of the sentence
* The factuality of the sentence 
* The meaning of AGI (if you are in the AI field.)

Even though there are benefits of knowing the source location of the data, the source link of data may not required to train neural networks <d-footnote> For example, the causal language modeling of GPT<d-cite key="brown2020language"/> and the masked language modeling of BERT do not require the source link to maximize the training objective. </d-footnote>. However, the source information is highly important when our objective is not performance but tracing the source of the information that the generative model provides. In short, 

> <text style="color:red"> Only for the training purpose, discarding irrelevant information is beneficial. But, we don't know made the information.  </text>

Within the progress of generative models, another training mechanism is required to ensure the safety issues for both models and data. 



---
Working...

---

## Source 

$$
p(k^*, x) = 
\begin{cases}
  1 & \text{ if } x \text{ is originated from  } k^* \\
  0 & \text{otherwise}
\end{cases}
$$


### Axis1: Abstract Matching Meaning 

Assume we have two generated texts $A, B$.

* Syntactic : the case when words are highly matched, but the meaning could be different
* Semantic  : the case when words are mismatched, but the grammar could be different

> We can compute the syntactic score by counting the number of same words. In the case of semantic score, we can embed with encoders such as BERT<d-cite key="devlin2018bert"/> the texts into vectors and obtain the cosine similarity. 

### Axis2: Copy Level

* Syntactic copy : 
* Semantic copy : 
* Novel : 




## Source Verification Problem 





## Related Work 


### Watermark

Watermark is a simple way to encode information in the representation and previously explored in diffusion models <d-cite key="fernandez2023stable"/>,<d-cite key="wen2023tree"/>. Although it is compelling way of encoding a secret key in the generated stuffs. It is only available for syntactic representation and unclear for the semantic meaning itself. 
