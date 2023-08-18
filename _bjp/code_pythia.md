---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: true
date: 2023-07-31
featured: true
title: 'Code Plan for Pythia Wrapper'
description: 'Implement API for Pythia model controls'
---


## Goal

The goal of pythia_wrapper is to provide useful tools to play with several Pythia models including training and analysis of neurons. 
To do this, we need API to handle several features of GPT models. The wrapper will play the goal API to play. 

## Features 

* generation of sentences 
* fine-tune the model with a new checkpoint. 
* given a hidden_representation or texts, provide output results including attention scores. 

## Runs 

* `generate.py` : given a model path, generate sentences with the model.
* `train.py` : given a model and dataset, fine-tune the model and save the checkpoint model 


## Shells

* `train.sh` : fine-tune a model and save the model. 

## Classes & Methods 

* `PythiaWrapper` 
  * __init__  : initialize the wrapper with pre-trained path and tokenizer.
  * generate  : given sentences or ids, generate the sentences 
  * forward_for_internals  : given ids, return forward results with additional outputs   
  * load_model : load a model from a checkpoint 
* `GPTData`
  * __init__  : initialize the data wrapper with loading the dataset 
  * prepare : prepare a dataset. (just hold a dataset)
  * get_data_iterator : get the iterator for the training dataset (return a iterator for the dataset. )

## Structure

```bash 
🗂 pytiawrap
    📃 __init__.py  # include PythiaWrapper and GPTData
    🗂 PythiaWrapper 
        📃 __init__.py
        📃 utils.py
    🗂 GPTData
        📃 __init__.py
        📃 download_dataset.py  
        📃 utils.py  

📃 train.py 
📃 generate.py 
```



