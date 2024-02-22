---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-09-06
featured: true
title: 'BERT Korean Symbol Representation Comparison'
description: 'Multi Agent Environment in Terminal'
img: 'https://onedrive.live.com/embed?resid=AE042A624064F8CA%211023&authkey=%21AA9m3GURpAxuLss&width=527&height=438'
github : 'https://github.com/deep-ing/gym-predator-prey'
---

> This analysis is conducted to analyze Korean vocabulary in **BERT embedding**.


## Introduction 

Language Modelings (LMs) has been widely applied to several NLP applications. 
When lots of NLP models are trained on several documents, the first step is to convert vocabulary into word embedding vectors. 
The vector representations in word embedding matrix, which are also called `"codes"` in `"codebook"`, are learned to minimize objectives such as 
casaul language modeling and masked language modeling.
When word embeddings are learned, we may expect similar representations if two words have commonality or relation.
To investigate such properties, we need to see the word representations in LMs. 
As such, this work conducts two visualizations

1. **Pair-wise Distance**  : which aims to compare representations of words
 
2. **Direct Representation Visualization**  : which aims to visualize vectors. 



## Visualization 


We visualize two sets of words (actually tokens) in Korean. 
These tokens are sequentially located tokens in BERT vocabulary. 

* **Consonants** : `[ㄱ ㄴ ㄷ ㄹ ㅁ ㅂ ㅅ ㅆ ㅇ ㅈ ㅊ ㅋ ㅌ ㅍ ㅎ ]`
* **Vowels** : `[ㅏ ㅐ ㅓ ㅔ ㅕ ㅗ ㅘ ㅛ ㅜ ㅝ ㅠ ㅡ ㅢ ]`


**Pairwise Disance** computes cosine similarity between two token vectors which I guess have 768 dimension. The direct representation visualizations plots only 14 and 26 dimensions respectively for the first and last directions.  The reason is to see position invariant features. As positional encoding is applied, the early dimensions affected more by the position while the later dimensions have liite disturbance from positional encoding.  

For example, if we have a word "Apple" and the word embedding for it, the positional dependent features are on the early parts and the semantic information of `"Apple"` is in the later location.  
 

#### Consonants (자음)


<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211022&authkey=%21ADpNs4WRFasrCSE&width=1378&height=396" style="width:140%;margin-left:0rem;border:1px;"  >


#### Vowels (모음)


<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211020&authkey=%21AOkQbKD62FLw9wE&width=1379&height=396" style="width:140%;margin-left:0rem;border:1px;"  >


#### Both Together (자음 + 모음)


<img src="https://onedrive.live.com/embed?resid=AE042A624064F8CA%211021&authkey=%21AGySUqLKAnNrpkM&width=1378&height=396" style="width:140%;margin-left:0rem;border:1px;"  >


---

## Reproduce the Result


<d-code block language="python">

import torch 
import seaborn as sns 
import matplotlib.pyplot as plt 
from transformers import BertTokenizerFast
from transformers import BertForMaskedLM

size ="base" # large
model = BertForMaskedLM.from_pretrained(f"bert-{size}-uncased")
tokenizer = BertTokenizerFast.from_pretrained(f"bert-{size}-uncased")


# 👀 collect vocab symbols
consonants =  list(range(29991, 30006)) 
vowels     =  list(range(30006, 30019))
consonants_chars = []
vowels_chars = []
for c in consonants:
    for k, v in tokenizer.get_vocab().items():  
        if c == v:
            print(f"{c} : {k}")
            consonants_chars.append(k)
print("------")
for c in vowels:
    for k, v in tokenizer.get_vocab().items():  
        if c == v:
            print(f"{c} : {k}")
            vowels_chars.append(k)



# 👀 visualize
def plot_embeddings(indices, w, suptitle, ):
    fig, axes = plt.subplots(1,3,figsize=(14,4))
    fig.suptitle(suptitle ,fontproperties=fontprop)
    v = w[torch.tensor(indices),:]
    pdist = torch.nn.PairwiseDistance(p=2, keepdim=True)
    output = torch.cdist(v, v, p=2)
    
    sns.heatmap(output, ax=axes[0], cmap='plasma')
    axes[0].set_title("Pairwise Distance")
    axes[0].set_xlabel("token")
    axes[0].set_ylabel("token")

    
    # 👀 position variant locations and invariant locations
    pos = v[:, :len(indices)]
    sns.heatmap(pos, cmap="Reds", ax=axes[1])
    axes[1].set_title("First 12 dimensions")
    axes[1].set_xlabel("dim")
    axes[1].set_ylabel("token")

    inpos = v[:, -len(indices):]
    sns.heatmap(inpos, cmap="Blues", ax=axes[2])
    axes[2].set_title("Last 12 dimensions")
    axes[2].set_xlabel("dim")
    axes[2].set_ylabel("token")
    plt.tight_layout()
    
w = model.bert.embeddings.word_embeddings.weight.to("cpu").detach()
plot_embeddings(consonants, w, 'Consonants \n' + " ".join(consonants_chars) , )
plot_embeddings(vowels, w, 'Vowels \n ' + " ".join(vowels_chars))
plot_embeddings(consonants+vowels, w, 
                'Consonants + Vowels  \n' + " ".join(consonants_chars) + "\n" + " ".join(vowels_chars), )

</d-code>