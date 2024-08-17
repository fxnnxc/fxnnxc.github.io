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


## Steering 의 목적

In-context 방식을 따르게
1. Few-shot example task --> query --> x 
2. Concept 향상 Styling 

---
3. Bias (x,y) pair 