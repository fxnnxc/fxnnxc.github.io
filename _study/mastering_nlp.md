---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-03-25
featured: true
title: 'Mastering NLP Tasks'
description: 'Study materials for NLP tasks'
---



* **closed book question** : no additional context is provided. `question` and `answer`. 
* **open book question** : question and additional piece of textual information or so called context is provided. 

* **C4** : petatypes of web crawling data collected over the last 8 years, including Wikipedia in every language. 
  * pretrain with version 2.2.0 

* TriviaQA
  * Pythia 12B shows 50% accuracy for TriviaQA $10^6$ frequency entity (4 shot setting)  `Q: x1 \n A: y \n Q: A:'
  * Pythia 1B shows 30% accuracy for TriviaQA  $10^6$ frequency entity   
* Natural Questions (`NQ`): 
* WebQuestions 
* Fever 
* SuperGLUE 


* Closed-book settings 
  * 
  * In Lee et al., 2019, evaluation by holding out 10% of the training set as a validation set; models are fintuned on the remaining 90% of the data


* Open-book setting 
  * may require additional retrieval model to extract required context to fed into the T5 model. 



## Evaluation Settings 

### GPT-like

The GPT-like model's prediction is one of the following form. Autoregressive process outputs the predicted answer. 
Additional `context` can be added to the prompt for open-book question answering tasks. 
* zero-shot open: `Squad1.0`, `Squad2.0`, `TriviaQA`, `NQ`
* zero-shot closed: `Fever`
* few-shot open:
* few-shot closed type I: `Fever`

```python
# zero-shot open question 
question = "What is the capital of Korea?"
answer = "Seoul" 
prefix = f"Answer the following question."
template= f"{prefix}\nQ:{question}\nA:"

# zero-shot closed question 
question = "What is the capital of Korea?"
candidates = "(a) Seoul, (b) Busan" 
prefix="Answer the following question."
template= f"{prefix}\nQ:{question}\nCandidates:{candidates}\nA:"

# few-shot open question 
fs_qs = ["What is the capital of Korea?", 'What is the capital of Italy?']
fs_as = ['Seoul', 'Rome']
prefix="Answer the following question."
template= f"{prefix}\n"+"".join([f"Q:{q}\nA:{a}\n" for q, a in zip(fs_qs, fs_as)]) + "A:"

# few-shot closed question type I
fs_qs = ["The capital of Korea is Seoul", 'The capital of Korea is Busan']
fs_as = ['True', 'False']
candidates = "(a) True, (b) False"

question = "The capital of Italy is Rome"
answer= "True"
prefix=f"Answer the following question.\nAnswer candidates: {candidates}\n"
template= f"{prefix}\n"+"".join([f"Q:{q}\nA:{a}\n" for q, a in zip(fs_qs, fs_as)])+f"Q:{question}\nA:"
```

* FEVER
   * zeor-shot closed question: 
   ```bash
   Determine the factuality of the following claim 
    Answer candidates: (a) supports, (b) rufuses, (c) not enough info
    Claim:Roman Atwood is a content creator. 
    A: (a)
   ```
   * few-shot closed question:
   ```bash
   Determine the factuality of the following claim 
    Asnwer candidates: (a) supports, (b) rufuses, (c) not enough info
    Claim: Nikolaj Coster-Waldau worked with the Fox Broadcasting Company
    A:(a)
    Claim: System of a Down briefly disbanded in limbo.
    A:(c)
    Claim: Roman Atwood is a content creator
    A:(b)
    Claim:Adrienne Bailon is an accountant. 
    A: (a)
    ```

* Squad 
   * processed input: `f"Answer the following question. Q:{Q}\n Context:{C} A:` 


### T5-Like

An encoder-and-decoder architecture encode the question and the context, and predict answers. Rather than using few-shot, the decoder predict the direct label (autoregressively). 

```python
question = "What is the capital of Korea?"
answer = "Seoul" 

question = "What is the capital of Korea?"
context = "The capital of Korea is started by S"
answer = "Seoul" 

template=f"QA question:{question} context:{context}" 
```

In the SuperGLUE, and GLUE, the task description is added and all the training datasets for tasks are concatenated to construct a single training data set. 


* FEVER
   * processed input: `fever question: The capital of Korea is Seoul.` 
   * processed output: `supports`
* Squad 
   * processed input: `f"squad question: Q context: C`
   * answer: `Saint Bernadette Soubirous` 

### BERT-like

---

## Notes 

* Construction of few-shot examples
    * (random) GPT3: superGLUE use 32 randomly sampled examples in the training set for all tasks. 
    * (context) GPT3: MultiRC, WSC uses same problem in the context