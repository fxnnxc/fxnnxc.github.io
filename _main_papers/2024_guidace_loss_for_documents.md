---
layout: distill
title:   'Memorizing Documents with Guidance in Large Language Models <strong>[IJCAI 2024]</strong>' 
date: 2024-06-23
giscus_comments : false
description: 
bibliography: all.bib
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
    - name: Jaesik Choi
      affiliations:
        name: KAIST
img: /assets/main_papers/2024_guidance_loss_fig_update2.png
author_names: <tag style="font-weight:800">Bumjin Park</tag>, Jaesik Choi
description: Memorizing documents with known location of GPT is an urgent task for the reliable and faithful generation. This work proposes document guidance loss to disentangle document knowledge in LLM. 
---

This post is a brief summary of the paper <br> "[Memorizing Documents with Guidance in Large Language Models]()" 

* **Code** is available at [Github](https://github.com/fxnnxc/document_guidance_loss_for_llm)🧑🏻‍💻 
* This work is accepted at [IJCAI 2024](https://ijcai24.org/)🍊
* Supplementary Materials [[pdf]() TBD]
* We release **Author Response** [[pdf](https://1drv.ms/b/s!Asr4ZEBiKgSujBZozaWGnG5RTfOy?e=HNG8cz)] for the readers interested in detailed discussions 


---

In AI, data plays a pivotal role in training.
Large language models (LLMs) are trained with massive amounts of documents, and their parameters hold document contents. Recently, several studies identified content-specific locations in LLMs by examining the parameters. 

Instead of the post hoc interpretation, we propose another approach, **document-wise memories**, which makes document-wise locations for neural memories in training. The proposed architecture maps document representation to memory entries and filters memory selections in the forward process of LLMs. Additionally, we propose document guidance loss, which increases the likelihood of text with document memories and reduces the likelihood with entries of other documents.

<img src="/assets/main_papers/2024_guidance_loss_fig_update2.png"  />




## Document-Wise Memory 


### Document-wise Memory Structure 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%2111113&authkey=%21AIpBI5W4DyyYpxY&width=1569&height=400" />

The document-wise memory entries have the following form. 
$$
\begin{equation}
    \operatorname{MLP}_\mathrm{doc}(x) =  V \Big( Key_\mathrm{Tok}(x) \odot  Key_\mathrm{Doc}(\mathcal{K}) \Big) + b_v  
\end{equation}
$$

The first key $ Key_\mathrm{Tok}(x)$ selects memories for the generation depending on the hidden representation $x$ in the language models. In contrast, the second key $ Key_\mathrm{Doc}(\mathcal{K})$ selects fixed entries of the memories for document $\mathcal{K}$.

### Document Guidance Loss 

To increase the likelihood of DocRep $\mathcal{K}$, the numerator part must be increased while the denominator part decreases.  This is proportional to maximizing the following equation
$$
\begin{equation}
    P_\theta(y_t\vert y_{<t}; \mathcal{K}_i) - \alpha P_\theta(y_t \vert y_{<t} )
\end{equation}
$$
The loss $\mathcal{L}$ takes the following form
$$
\begin{equation}
    \mathcal{L} = \mathcal{L}_{CE}(\hat{y}^{\mathcal{K}_i}, y) + \alpha \vert  \tau - \mathcal{L}_{CE}(\hat{y}^{\mathcal{K}^{-}}, y)  \vert
\end{equation}
$$
where $$ \mathcal{L}_{CE}(\hat{y}^{\mathcal{K}}, y)$$ is the cross entropy loss of passage $y$ and conditional generation $\hat{y}^{\mathcal{K}} \sim P_\theta(\cdot\vert \cdot, \mathcal{K})$. The right part is the forgetting loss with negative DocRep $\mathcal{K}^{-}$ and the expected loss $\tau$ for $P_{low}$. WLOG, we assume that $\tau$ is constant. 


## Memory Selection and Perplexity

Selected memories for documents have the following properties.

1. The **perplexity** changes smoothly for **the smooth change of the document representation**. 
2. The activation plays an **inductive bias** in memory selection.
3. The memory size correlates to **the document length**.

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%2111114&authkey=%21AH53z9ZObnc3kQQ&width=1144&height=932" />

## Measuring Separability

When recalling with document memories, ReLU and Tanh activations show how recall of the original document. Therefore, the trained memories stores more separable document contents! 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%2111116&authkey=%21APCRWUZMnzAqiFs&width=1353&height=549"  />

## Caveats of Nonlinear Memory Selection 

### More Dead Neurons

We observed that nonlinear memory selections do not work well with the proposed guidance loss. We compare 1 (linear case) to 4 layers with ReLU internal and final activations, adding $\mathrm{MLP}_\mathrm{Doc}$ after the last encoder layer. The models are trained with 10 documents, $\alpha=1.0, \tau=4.5$, and random negative DocReps. We observed **dead neurons (not activated)**.

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%2111115&authkey=%21AOa94jRy_1QF1XI&width=1469&height=692" />


### Quantitative Verification 

To quantitatively evaluate memory selections, we train linear and nonlinear cases for five seeds and measure $L_0$, which is the number of non-zero entries, $L_2$ norm, and pairwise distance, which is $\Vert g(\mathcal{K}_i) - g(\mathcal{K}_j) \Vert _2$ for two DocReps $\mathcal{K}_i$ and $\mathcal{K}_j$, and normalized pairwise distance by $L_2$ norm of memory entries. All metrics are averaged over ten documents. The number of nonzero entries ($L_0$) increases for all cases, meaning the rank of memory usage increases. However, the normalized pairwise distance decreases as the layer depth increases. The linear memory provides more different memory entries than the nonlinear cases. The large gap between linear and nonlinear indicates the limitation of guidance loss with nonlinear cases. 

<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%2111117&authkey=%21AFulGmuI-UF2MtI&width=1832&height=383"  />


## Conclusion 

Storing documents in the traceable locations of LLMs is a crucial research topic. This paper studies document-wise memories in LLMs. We propose document guidance loss to entangle document contents and document memories, encouraging different entries for documents. We also provide a theoretical view of memory selections with metric spaces and continuity assumptions. The experimental results show that the proposed guidance loss provides different memory entries with linear memory selection while leaving nonlinear memory selection as an open problem. 