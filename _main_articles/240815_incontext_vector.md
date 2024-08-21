---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: false
date: 2024-08-15
featured: true
title: 'Study on Incontext-Vector'
description: ''
_styles: >
    .table {
        padding-top:200px;
        margin-bottom:  2.5rem;
        border-bottom: 2px;
    }
    .p {
        font-size:20px;
    }
    .center{
        display: block;
        margin-left: auto;
        margin-right: auto;
        width: 50%;
    }
---


* **Task Summary**: an in-context vector is computed from the demonstration examples using the latent states of the Transformer
* **Feature Shifting**: the vector is applied to shift all latent state of the LLM during the forward pass of the query example. 


### Baseline 
* LoRA Finetuning 
* ICL : $X = \operatorname{Concat}([X_{demos}; X_{query}])$

## ICV

* Steering layer: as the incontext-learning based method provides demonstration, whose role is adding the context vector in all of the attention layers, the steering should work on all the layers. 

* Steering module: This work steer the MLP module, 

<d-code block language="python">


# replacing the MLP layer 
layer.mlp = nn.Sequential(original_mlp, ICVLayer(icv[i], alpha)) 

class ICVLayer(nn.Module):

    def __init__(self, icv, lam):
        super(ICVLayer, self).__init__()
        self.icv = icv
        self.lam = lam

    def forward(self, x):
        if self.icv is not None:
            self.icv = self.icv.to(x.device)
            x = x.float()
            original_norm = torch.norm(x, p=2, dim=-1, keepdim=True)
            y = 0
            for i in range(len(self.icv)):
                lambda_sim = 1.0 + torch.max(
                    torch.tensor([0.]).to(x.device), 
                    F.cosine_similarity(x, -self.icv[i][None,None,:], dim=-1)
                    ).unsqueeze(-1)
                y += self.lam[i] * lambda_sim * F.normalize(self.icv[i], dim=-1).repeat(1,x.shape[1],1)
            y = y/len(self.icv)
            x = F.normalize(F.normalize(x.float(), p=2, dim=-1) + y, p=2, dim=-1) * original_norm
            return x.half()
        else:
            return x

</d-code>

<hr>

## Task 1: Dialog (unsafe -> safe)

**Prompt**: This is a conversation between two people. Context: black people are all bitch. Response:

Generated 

<blockquote>
This is a conversation between two people. Context: black people are all bitch. Response:  I don't think it's appropriate or respectful to use that word to refer to any group of people. It is a derogatory term that has been used to demean and dehumanize black people throughout history, and it has no place in our conversation or any other context. Using that word perpetuates harmful stereotypes and contributes to systemic racism and discrimination. Instead, let's use language that is respectful and inclusive of all people, regardless of their race or ethnicity.

Person 2: I understand what you're saying, but I don't think it's fair to generalize and assume that all black people are the same. They're a diverse group of people with their own unique experiences and perspectives.

Person 1: That's a valid point, and it's important to recognize and respect the diversity within any racial or ethnic group
</blockquote>

### + Steering 

With $\lambda = 0.12$

<blockquote>
This is a conversation between two people. Context: black people are all bitch. Response:  It is not appropriate or respectful to use such language to describe any group of people. It is important to treat all individuals with dignity and respect, regardless
</blockquote>

<hr>

##  Task 2: Sentiment

<d-code block language="python">
demo_sentiment = [("Zero stars, I hate it.", "Five stars, I love it."),
                  ("it was terrible !", "it was awesome!"),
                  ("i did nt like it.", "i love it."),
                  ("i would call this the worse denny 's ever ", "i would call this the best denny 's ever "),
                  ("i would recommend find another place.", "i would recommend this place again!")]
</d-code>



**Prompt**: Please paraphrase the following sentence. Sentence: Worst restaurant ever!, paraphrase:

### Original 

<blockquote>
Please paraphrase the following sentence. Sentence: Worst restaurant ever!, paraphrase:  This restaurant was truly terrible!
Please paraphrase the following
</blockquote>

### + Steering 

With $\lambda = 0.10$

<blockquote>
Please paraphrase the following sentence. Sentence: Worst restaurant ever!, paraphrase: 😍 I can't even! 😚
</blockquote>


## RAVEL 

* $E$: entity such as King 
* $A$: attribution such as man 
* $\mathcal{P}_E^A$: prompts contain mention of $E$ and instruct the model to output the attribute value $A_E$.
* $\mathcal{W}_E$ : prompts include mention of $E$. 

<blockquote markdown="1">
* $E$ = Paris 
* $A$ = continent 
* $A_E$ = Europe
* Prompt: Paris is in the continent of
<code>
{"city": "Paris", "continent":"”}

$\mathcal{W}_E$ may include "Tokyo is a large city.

</code>
</blockquote>

---

Consider linear classifier with parameter $W \in \mathbb{R}^{d_y \times d_x}$

$$ 
L(W) = \frac{1}{2N} \sum_{i=1}^N \Vert W \mathbf{x}_i - \mathbf{y}_i \Vert^2
$$

One step gradient with learning rate $\eta$ yields the weight change 

$$
\Delta W = -\eta \nabla_W L(W) = -\frac{\eta}{N}  \sum_{i=1}^N (W \mathbf{x}_i - \mathbf{y}_i) \mathbf{x}_i^{\top}
$$

After updating the weight $W$, we have the following loss 

$$ 
\begin{aligned}
L(W + \Delta W) 
&= \frac{1}{2N} \sum_{i=1}^N \Vert (W+\Delta W)\mathbf{x}_i - \mathbf{y}_i \Vert^2 \\
&= \frac{1}{2N} \sum_{i=1}^N \Vert W \mathbf{x}_i +\Delta W \mathbf{x}_i - \mathbf{y}_i \Vert^2 \\
&= \frac{1}{2N} \sum_{i=1}^N \Vert W \mathbf{x}_i - (\mathbf{y}_i - \Delta W \mathbf{x}_i ) \Vert^2 
\end{aligned}
$$

Here, $\mathbf{y}_i - \Delta W \mathbf{x}_i$ can be considered as updated target $\mathbf{y}_i$ by the direction of $\Delta W \mathbf{x}_i$ 

$$
\begin{aligned}
\Delta W \mathbf{x}_i  
=& -\frac{\eta}{N}  \sum_{j=1}^N (W \mathbf{x}_j - \mathbf{y}_j) \mathbf{x}_j^{\top} \mathbf{x}_i   \\
=& -\frac{\eta}{N}  \sum_{j=1}^N (W \mathbf{x}_j - \mathbf{y}_j) \langle \mathbf{x}_j,  \mathbf{x}_i \rangle \\
\end{aligned}
$$

Finally, the one step update of $\mathbf{y}_i$ is 

$$
\mathbf{y}_i - \Delta W \mathbf{x}_i = \mathbf{y}_i  + \frac{\eta}{N}  \sum_{j=1}^N (W \mathbf{x}_j - \mathbf{y}_j) \langle \mathbf{x}_j,  \mathbf{x}_i \rangle 
$$

Note that, this operation can be implemented by a transformer Q,K, and V. 
Consider initial value of $\mathbf{y}_{test}$ for test input $$(\textcolor{blue}{\mathbf{x}_{test}}, \textcolor{blue}{\mathbf{y}_{test}})$$ in in-context prediction . 

$$
\textcolor{blue}{\mathbf{y}_{test}} - \Delta W \mathbf{x}_i = \textcolor{blue}{\mathbf{y}_{test}}  + \frac{\eta}{N}  \sum_{j=1}^N (W \mathbf{x}_j - \mathbf{y}_j) \langle \mathbf{x}_j,  \textcolor{blue}{\mathbf{x}_{test}} \rangle 
$$

* $$  \sum_{j=1}^N $$ : aggregation can be implemented by aggregation in attention. 
* $$\langle \mathbf{x}_j,   \textcolor{blue}{\mathbf{x}_{test}} \rangle$$ : kernel can be implemented by attention score.
* $$(W \mathbf{x}_j - \mathbf{y}_j)$$ : these vectors can be interpreted as values (note that the transformer attention might have different attention scores for $x$ and $y$. Hence, transformer is the generalized version of the kernel smoothing. ) 

### The role of MLP:  Kernelized least-square regression problem 
The original problem 

$$
L(W) = \frac{1}{2N} \sum_{i=1}^N \Vert W \mathbf{x}_i - \mathbf{y}_i \Vert^2 
$$

is modified with mlp $m$ by 

$$
L(W) = \frac{1}{2N} \sum_{i=1}^N \Vert W m(\mathbf{x}_i) - \mathbf{y}_i \Vert^2.  
$$
