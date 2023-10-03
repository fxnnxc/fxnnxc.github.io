---
layout: share-distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2023-08-24
featured: true
img: 
title: '[Experiment Note] <br> Source Identification : GPT last hidden probing '
description: 'GPT 마지막 표현 공간으로부터 Source 구분 문제'
_styles: >
    .table {
        padding-top:200px;
        margin-bottom: 2.5rem;
        border-bottom: 2px;
    }
    .p {
        font-size:20px;
    }

---

* W&B : https://wandb.ai/bumjin/sip-num_words_comparison?workspace=user-bumjin
* Github Code source_identification_problem [experiment 1.0.1](https://github.com/fxnnxc/source_identification_problem/tree/experiment1.0.1)


Hyperparameter is the for num_words in [4 8 16 32]

<d-code language='python'>


9 👾 FLAGS
10   datetime: 10_02_141342
11   data: wikitext-2-v1
12   data_cache_dir: /data/EleutherAI_wikitext
13   num_words: 4
14   max_tokens: 32
15   lm_batch_size: 8
16   num_first_labels: 10
17   num_samples_per_label: 1000
18   min_samples_per_label: 1000
19   datasets_split_ratio: 0.8
20   exp_name: num_words_comparison
21   test: False
22   lm_model: pythia
23   lm_model_size: 1b
24   lm_model_path: /data/EleutherAI
25   lm_num_gpus: 2
26   lr: 0.001
27   batch_size: 8
28   hash_type: binary
29   device: cuda:0
30   num_warmup_steps: 1000
31   num_epochs: 1000
32   num_train_steps: 69000
33   num_eval: 10
34   num_save: 10
35   hash_model_type: linear
36   hash_batch_size: 128
37   linear_hidden_size: 256
38   linear_activation: relu
39   linear_n_layers: 6
40   rnn_hidden_size: 128
41   rnn_n_layers: 2
42   bert_num_hidden_layers: 12
43   bert_replace_to_bert: False
44   task: train_hash
45   project_name: sip-num_words_comparison
46   save_dir: /data4/bumjin/sip-num_words_comparison/train_hash/num_words_comparison/wikitext-2-v1/1b/binary_linear/10_02_141342
47   num_binaries: 4
48   num_labels: 11

</d-code>