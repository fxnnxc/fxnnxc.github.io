---
layout: distill
title: 'Mathematical Synthetic Token Dataset'
date: 2023-09-04
description: How I make synthetic dataset for GPT training. 
img: 'https://drive.google.com/uc?export=view&id=1m3GJfGpet3kyijkHRVr-5QK23ijtcKxh'
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: false
---


## Introduction 


Recently, the mechanism of transformer architecture has been studied and several properties  are discovered. 
To investigate the mechanism, all the processes of training should be revealed. However, as most foundation models have trained on complex NLP domains, the exact underlying states are unclear. To make the procedures clear, the dataset must be under control. In this project we develop a synthetic dataset for training transformer models. In detail, we build mathematical token sequences with customized rules and easy labels. 


## Design

We first build the easiest token dataset and inherit the class to make a mathematical token dataset. The mathematical token dataset is further used to make a compositional dataset which combines several mathematical operational datasets. That is, we make an individual dataset such as addition and subtraction with a user-defined number of digit symbols. Then, a compositional dataset further combines  several operational datasets. 

Note that these mathematical synthetic datasets have no label, but a sequence of tokens to represent any operations. To generate labels, we define “domain”. A domain is a source of sequences assumed to have labels. As we constructed datasets with operations (+, -, *, //), we assign synthetic labels to each sequence. For example, if the label is “whether a sequence has zero” or “whether a sequence does not have zero” would be assigned if the condition is met. 



## Tokens 

We have symbolic tokens which indicate

* digit : numbers `0 ~ N`
* special tokens  : `+ - * //`



<d-code block language="python">


def _make_special_tokens(self):
    # 📌 implemented
    # 👀 vocab_len is number of tokens + number of special_tokens

    self.equal_token     = self.num_symbols + 0
    self.mod_equal_token = self.num_symbols + 1 
    self.add_token       = self.num_symbols + 2
    self.multiply_token  = self.num_symbols + 3
    self.subtract_token  = self.num_symbols + 4
    self.divide_token    = self.num_symbols + 5
    self.negative_token  = self.num_symbols + 6
    self.eos_token = self.num_symbols + 7
    self.num_special_tokens = 8
    self.vocab_len = self.num_symbols + self.num_special_tokens
        
    self.token_to_symbol = {
        i:str(i) for i in range(self.num_symbols)
    }
    self.token_to_symbol[self.equal_token] = "="
    self.token_to_symbol[self.mod_equal_token] = "=(m)"
    self.token_to_symbol[self.add_token] = "+"
    self.token_to_symbol[self.multiply_token] = "*"
    self.token_to_symbol[self.subtract_token] = "-"
    self.token_to_symbol[self.divide_token] = "/"
    self.token_to_symbol[self.negative_token] = "-"
    self.token_to_symbol[self.eos_token] = "[eos]>"
    self.symbol_to_token = {v: k for k, v in self.token_to_symbol.items()}

</d-code>


---


## Examples 

* **Addition** [[go_to_the_link](#addition)] : a single addition operation is used. 
* **Composition** [[go_to_the_link](#composition-addsubtractmultiplydivide)] : four types of operations are combined to construct single dataset.
* **Modulo** [[go_to_the_link](#modulo--composition)] : equation is replaced by modulo equation. 
* **Domain** [[go_to_the_link](#domain-labels)] : assign labels to each sequence

---


📌 Example 1
#### Addition 

<d-code block language="python">


from green.tokens import print_dataset
from green.tokens import MathematicalTokenDatasetV1
train_dataset = MathematicalTokenDatasetV1(True, num_symbols=5,  seed=0, modulo=False, modulo_int=None)
valid_dataset = MathematicalTokenDatasetV1(False, num_symbols=5, seed=0, modulo=False, modulo_int=None)

print_dataset(train_dataset, valid_dataset)

# 🖨️ these lines are the printed results 
------- Train: True
------- Length: 10
------- num_symbols: 5
------- vocab_len 13
sequence indices : [array([ 1,  6,  8,  9, 14,  4,  2, 13, 10,  7])]
tokens / decoded
(0, 7, 1, 5, 1) 0 + 1 = 1
(1, 7, 1, 5, 2) 1 + 1 = 2
(1, 7, 3, 5, 4) 1 + 3 = 4
(2, 7, 0, 5, 2) 2 + 0 = 2
(4, 7, 0, 5, 4) 4 + 0 = 4
(0, 7, 4, 5, 4) 0 + 4 = 4
(0, 7, 2, 5, 2) 0 + 2 = 2
(3, 7, 1, 5, 4) 3 + 1 = 4
(2, 7, 1, 5, 3) 2 + 1 = 3
(1, 7, 2, 5, 3) 1 + 2 = 3
------- Train: False
------- Length: 5
------- num_symbols: 5
------- vocab_len 13
sequence indices : [[0, 3, 5, 11, 12]]
tokens / decoded
(0, 7, 0, 5, 0) 0 + 0 = 0
(0, 7, 3, 5, 3) 0 + 3 = 3
(1, 7, 0, 5, 1) 1 + 0 = 1
(2, 7, 2, 5, 4) 2 + 2 = 4
(3, 7, 0, 5, 3) 3 + 0 = 3

</d-code>

---

📌 Example 2
#### Composition (Add+Subtract+Multiply+Divide)

<d-code block language="python">
from green.tokens import MathematicalCompositionalDatasetV4
train_dataset = MathematicalCompositionalDatasetV4(True, num_symbols=3,  seed=0, modulo=False, modulo_int=None)
valid_dataset = MathematicalCompositionalDatasetV4(False, num_symbols=3, seed=0, modulo=False, modulo_int=None)


print_dataset(train_dataset, valid_dataset)


# 🖨️ these lines are the printed results
------- Train: True
------- Length: 19
------- num_symbols: 3
------- vocab_len 11
sequence indices : [array([5, 2, 1, 3]), array([7, 2, 1, 4, 8, 6]), array([6, 2, 1, 7, 3]), array([5, 2, 1, 3])]
tokens / decoded
(2, 5, 0, 3, 2) 2 + 0 = 2
(0, 5, 2, 3, 2) 0 + 2 = 2
(0, 5, 1, 3, 1) 0 + 1 = 1
(1, 5, 0, 3, 1) 1 + 0 = 1
(2, 7, 1, 3, 1) 2 - 1 = 1
(0, 7, 2, 3, 9, 2) 0 - 2 = - 2
(0, 7, 1, 3, 9, 1) 0 - 1 = - 1
(1, 7, 1, 3, 0) 1 - 1 = 0
(2, 7, 2, 3, 0) 2 - 2 = 0
(2, 7, 0, 3, 2) 2 - 0 = 2
(2, 6, 0, 3, 0) 2 * 0 = 0
(0, 6, 2, 3, 0) 0 * 2 = 0
(0, 6, 1, 3, 0) 0 * 1 = 0
(2, 6, 1, 3, 2) 2 * 1 = 2
(1, 6, 0, 3, 0) 1 * 0 = 0
(2, 8, 2, 3, 1) 2 / 2 = 1
(1, 8, 1, 3, 1) 1 / 1 = 1
(0, 8, 2, 3, 0) 0 / 2 = 0
(1, 8, 2, 3, 0) 1 / 2 = 0
------- Train: False
------- Length: 10
------- num_symbols: 3
------- vocab_len 11
sequence indices : [[0, 4], [0, 3, 5], [0, 4, 5], [0, 4]]
tokens / decoded
(0, 5, 0, 3, 0) 0 + 0 = 0
(1, 5, 1, 3, 2) 1 + 1 = 2
(0, 7, 0, 3, 0) 0 - 0 = 0
(1, 7, 0, 3, 1) 1 - 0 = 1
(1, 7, 2, 3, 9, 1) 1 - 2 = - 1
(0, 6, 0, 3, 0) 0 * 0 = 0
(1, 6, 1, 3, 1) 1 * 1 = 1
(1, 6, 2, 3, 2) 1 * 2 = 2
(0, 8, 1, 3, 0) 0 / 1 = 0
(2, 8, 1, 3, 2) 2 / 1 = 2
</d-code>

---

📌 Example 3
#### Modulo + Composition 

<d-code block language="python">

from green.tokens import MathematicalCompositionalDatasetV2
train_dataset = MathematicalCompositionalDatasetV2(True, num_symbols=3,  seed=0, modulo=True, modulo_int=2)
valid_dataset = MathematicalCompositionalDatasetV2(False, num_symbols=3, seed=0, modulo=True, modulo_int=2)


print_dataset(train_dataset, valid_dataset)


# 🖨️ these lines are the printed results
------- Train: True
------- Length: 12
------- num_symbols: 3
------- vocab_len 11
sequence indices : [array([7, 2, 1, 4, 8, 6]), array([7, 2, 1, 4, 8, 6])]
(2, 5, 1, 4, 2, 1) 2 + 1 =(m) 2 1
(0, 5, 2, 4, 2, 0) 0 + 2 =(m) 2 0
(0, 5, 1, 4, 2, 1) 0 + 1 =(m) 2 1
(1, 5, 1, 4, 2, 0) 1 + 1 =(m) 2 0
(2, 5, 2, 4, 2, 0) 2 + 2 =(m) 2 0
(2, 5, 0, 4, 2, 0) 2 + 0 =(m) 2 0
(2, 6, 1, 4, 2, 0) 2 * 1 =(m) 2 0
(0, 6, 2, 4, 2, 0) 0 * 2 =(m) 2 0
(0, 6, 1, 4, 2, 0) 0 * 1 =(m) 2 0
(1, 6, 1, 4, 2, 1) 1 * 1 =(m) 2 1
(2, 6, 2, 4, 2, 0) 2 * 2 =(m) 2 0
(2, 6, 0, 4, 2, 0) 2 * 0 =(m) 2 0
------- Train: False
------- Length: 6
------- num_symbols: 3
------- vocab_len 11
sequence indices : [[0, 3, 5], [0, 3, 5]]
(0, 5, 0, 4, 2, 0) 0 + 0 =(m) 2 0
(1, 5, 0, 4, 2, 1) 1 + 0 =(m) 2 1
(1, 5, 2, 4, 2, 1) 1 + 2 =(m) 2 1
(0, 6, 0, 4, 2, 0) 0 * 0 =(m) 2 0
(1, 6, 0, 4, 2, 0) 1 * 0 =(m) 2 0
(1, 6, 2, 4, 2, 0) 1 * 2 =(m) 2 0

</d-code>


---

📌 Example 4
#### Domain Labels 

Consider the following domains 


<d-code block language="python">

# 🌟 domain labels

# 0: when the first symbol is odd 
# 1: when the first symbol is even
# 2: when the first symbol and the second symbol are both odd 
# 3: when the first symbol and the second symbol are both even
# 4: when zero is included in the sequence
# 5: when zero is not included in the sequence
</d-code>


<d-code block language="python">

from green.tokens import MathematicalDomainDatasetV1 
from green.tokens import print_domain_dataset

train_domain = MathematicalDomainDatasetV1(train_dataset)
valid_domain = MathematicalDomainDatasetV1(valid_dataset)


print_domain_dataset(train_domain, valid_domain)


# 🖨️ these lines are the printed results

------- Train: True
------- Length: 10
------- num_symbols: 3
------- vocab_len 11
sequence indices : [array([5, 2, 1, 3]), array([7, 2, 1, 4, 8, 6])]
(2, 5, 0, 3, 2) 2 + 0 = 2  domain: [1, 3, 4]
(0, 5, 2, 3, 2) 0 + 2 = 2  domain: [1, 3, 4]
(0, 5, 1, 3, 1) 0 + 1 = 1  domain: [1, 4]
(1, 5, 0, 3, 1) 1 + 0 = 1  domain: [0, 4]
(2, 7, 1, 3, 1) 2 - 1 = 1  domain: [1, 5]
(0, 7, 2, 3, 9, 2) 0 - 2 = - 2  domain: [1, 3, 4]
(0, 7, 1, 3, 9, 1) 0 - 1 = - 1  domain: [1, 4]
(1, 7, 1, 3, 0) 1 - 1 = 0  domain: [0, 2, 4]
(2, 7, 2, 3, 0) 2 - 2 = 0  domain: [1, 3, 4]
(2, 7, 0, 3, 2) 2 - 0 = 2  domain: [1, 3, 4]
------- Train: False
------- Length: 5
------- num_symbols: 3
------- vocab_len 11
sequence indices : [[0, 4], [0, 3, 5]]
(0, 5, 0, 3, 0) 0 + 0 = 0  domain: [1, 3, 4]
(1, 5, 1, 3, 2) 1 + 1 = 2  domain: [0, 2, 5]
(0, 7, 0, 3, 0) 0 - 0 = 0  domain: [1, 3, 4]
(1, 7, 0, 3, 1) 1 - 0 = 1  domain: [0, 4]
(1, 7, 2, 3, 9, 1) 1 - 2 = - 1  domain: [0, 5]

</d-code>