---
layout: share-distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-03-28
featured: true
title: 'Does Adaptation of Neural Memory help factual prediction? (Korean)'
description: '정보 암기를 위한 추가적인 내부 메모리는 모델 예측에 도움이 되는가?'
---

## Does Adaptation of Neural Memory help factual prediction? 

* 🗓️ **Experiment Date** : 24.03.28 
* 🌸 **Scope** : Few-shot classification for large language models with memorization adaptation. We use FEVER dataset 99 pages and related validation question <br> (#samples: 3045). 
* 🧑🏻‍💻 **Code**:[llm:v24.03.29_memorization](https://github.com/fxnnxc/llm/tree/v24.03.29_memorization)
* 😮 **Status** : Running Experiments 
  - [x] &nbsp; *Documentation of Settings*
  - [x] &nbsp; *Running Experiments*
  - [x] &nbsp; *Record Results*
  - [ ] &nbsp; *Analysis Results* 

---

## 1. Memory Adapted LLMs

GPT-like LLM은 기본적으로 few-shot learning이 가능하다. 질문에 대한 예시를 바탕으로 비슷하게 예측을 준다<d-footnote>T5 형태의 모델은 질문을 인코더로 입력하고 예측을 디코더에서 할 수 있으므로, few-shot 방식이 필요하지 않다. GPT-like 모델은 문장을 암기하는 형태로 정보를 저장하며, 예측 성능을 높이기 위해서는 few-shot 방식을 사용해야 한다.</d-footnote>. 뉴럴 메모리를 학습하는 방식은 GPT블록 출력에 대해서 residual connection으로 추가적인 표현을 더하는 방식이다. 본 실험에서 비교하는 것은 메모리에 저장하는 정보를 다르게 하는 것이다. 저장하는 정보는 두 가지 종류다. 

1. Document Contents : LLM은 문서에 대한 정보를 추가적으로 외운다. 이 때 전체 파라미터에 저장하기보다 기존 파라미터를 보존할 상태로 추가적인 파라미터로 연산한다. 
2. Template Memorization : Factual Answer에 대한 template을 학습한다. 이는 LLM이 template 형태의 대답에 대해서 제대로 반응하지 못하는 문제를 해결함과 동시에 질의-대답에 대한 관계를 학습하도록 돕는다. 

## 2. 데이터 준비 파이프라인 

Notations: $N_{full}, N_{top}, N_{trunc}$, are the number of all evidence documents in fever validation, number of top $K$ documents, and number of truncated documents by removing invalid documents, respectively.  

1. validation에 Q-A데이터를 각 문서에 대한 Q-A로 정리한다. 
2. Q-A가 많은 순서대로 정렬하여, top-N개의 문서를 고른다. $$
3. 문서 길이가 0이거나, 학습 Q-A 데이터에 문서가 존재하지 않는 경우, 해당 문서를 제외한다.

### Few-shot Template

We use the following template for few-shot inference. 

```bash 
Determine the factuality of the following claims 
Answer candidates: (a) supports, (b) refutes, (c) not enough info
Claim: Roman Atwood is a content creator.
A: (a)
Claim: System of a Down briefly disbanded in limbo.
A: (b)
Claim: Adrienne Bailon is an accountant.
A:
```

### Few-shot setting 

문서 정보 학습에 대한 few-shot setting은 문서에 해당하는 Q-A Pair를 학습하는 방식으로 진행된다. 
만일 Q-A pair에 충분한 데이터가 없을 경우, support, refutes, not enough info에 대해서 추가적인 데이터를 더해준다. 


```bash
Nikolaj Coster-Waldau worked with the Fox Broadcasting Company.
Roman Atwood is a content creator.
Tetris has sold millions of physical copies.
The Boston Celtics play their home games at TD Garden.
The Ten Commandments is an epic film.

System of a Down briefly disbanded in limbo.
Beautiful reached number two on the Billboard Hot 100 in 2003.
Afghanistan is the source of the Kushan dynasty.
Marilyn Monroe worked with Warner Brothers.
United Nations was relocated to the United States in 1945.
Keith Urban is a person who sings.

Adrienne Bailon is an accountant.
Stranger Things is set in Bloomington, Indiana.
Puerto Rico is not an unincorporated territory of the United States.
Peggy Sue Got Married is a Egyptian film released in 1986.
Andy Roddick lost 5 Master Series between 2002 and 2010.
Willie Nelson dropped out of college after three years.
```

## 3. Model Candidates  

1. **Pretrained** : 🤗 Huggingface 에서 받은 기본 모델 파라미터 상태 
2. **Document Memorization (Doc)** : 추가적인 메모리에 evidence Wiki page를 암기하도록 학습한다. 
3. **Template Memorization (Temp)** : 추가적인 메모리에 Wiki page와 관련된 학습 Q-A를 few-shot 형태로 암기한다.  
4. **Document + Template Memorization (DocTemp)** : 추가적인 메모리에 두 가지를 모두 학습한다. 

메모리를 저장하는 방법은 블록의 아웃풋에 대해서 추가적인 표현을 학습하여 residual에 더하는 방식으로 진행된다. 
메모리의 사이즈는 다음과 같다.

<d-code block language='python'>
class MemMdoule(nn.Module):
    def __init__(self, hidden_dim, model_dim):
        super().__init__()
        self.up = layer_init(nn.Linear(model_dim, hidden_dim))
        self.act = nn.GELU()
        self.down = layer_init(nn.Linear(hidden_dim, model_dim))
        self.ln = nn.LayerNorm(model_dim)
        self.gate = nn.Parameter(torch.tensor([1.0]))
        
    def forward(self, x):
        x= self.up(x)
        x = self.act(x)
        x = self.down(x)
        x = self.ln(x) * self.gate
        return x
</d-code>


## 4. 학습 세팅 

```python
optimizer='adam'
lr_scheduler='cosine'
batch_size=4
lr=0.001
num_epochs=25
num_warmup_steps=10
optimizer_weight_decay=0.05  # 메모리가 최대한 적은 영향을 가지도록
grad_clip_coeff=10.0
hook_memory_dim=128
memory_module location=70~80% location of transformer blocks 
```

## 5. 학습 결과 

We answer the following questions.
1. Does few-shot examples help the factual prediction without memorization fine tuning?
2. Does memorization of document pages help the prediction?
3. Does memorization of templates help the prediction?
4. Does memorization of both show the best performance?

### 1. Fever  Pages F1 Score (Pretrained)

|Model            |  zero-shot| 1-shot    | 2-shot     | 3-shot     | 4-shot     | 5-shot     |
|-----------------|-----------|-----------|------------|------------|------------|------------|
llama2 13b        |  0.013    | 0.460 | 0.410 | 0.417 | 0.472 | **0.476** | 
llama2 7b         |  0.018    | **0.435** | 0.354 | 0.368 | 0.367 | 0.364 | 
llama2_chat 13b   |  0.010    | 0.556 | 0.389 | 0.496 | 0.552 | **0.578** | 
llama2_chat 7b    |  **0.392**    | **0.636** | 0.519 | 0.463 | 0.408 | 0.393 | 
|---|---|---|---|---|
pythia 12b   |  0.000 | **0.366** | 0.118 | 0.119 | 0.113 | 0.083 | 
pythia 6.9b  |  0.021 | 0.163 | **0.280** | 0.175 | 0.106 | 0.206 | 
pythia 2.8b  |  0.006 | 0.026 | 0.097 | 0.056 | 0.108 | **0.132** | 
pythia 1.4b  |  0.059 | 0.009 | 0.139 | 0.177 | **0.275** | 0.176 | 
pythia 1b    |  0.000 | 0.000 | 0.001 | 0.002 | 0.011 | 0.013 | 
pythia 410m  |  0.000 | 0.010 | 0.000 | 0.000 | 0.000 | 0.000 | 
pythia 160m  |  0.000 | 0.000 | 0.000 | 0.000 | 0.000 | 0.000 | 
pythia 70m   |  0.000 | 0.000 | 0.000 | 0.000 | 0.000 | 0.000 | 


#### Fever 99 Pages F1 Score (template memo - 1 shot, weight decay 0.05)

|Model            |  1-shot (base) | Document      | Template (Train Q-A)   | Doc + Template  |
|-----------------|----------------|---------------|------------|-----------------|
llama2 13b        |  0.460         | **0.476** | 0.465 | 0.462 |
llama2 7b         |  0.435         | 0.439 | **0.445** | 0.438 |
llama2_chat 13b   |  0.556         | 0.561 | **0.562** | 0.558 | 
llama2_chat 7b    |  0.636         | **0.650** | 0.647 | 0.646 |
|---|---|---|---|---|
pythia 12b   | 0.366 | 0.366 | 0.366 | 0.366 | 
pythia 6.9b  | 0.163 | 0.163 | 0.163 | 0.163 |  
pythia 2.8b  | 0.026 | 0.026 | 0.026 | 0.026 |
pythia 1.4b  | 0.009 | 0.009 | 0.009 | 0.009 |
pythia 1b    | 0.000 | 0.000 | 0.000 | 0.000 |
pythia 410m  | 0.010 | 0.010 | 0.010 | 0.010 |
pythia 160m  | 0.000 | 0.000 | 0.000 | 0.000 |
pythia 70m   | 0.000 | 0.000 | 0.000 | 0.000 |

#### Weight Decay 0.01  (0.05 --> 0.01)

|Model            |  1-shot (base) | Best Performance with 0.05 Weight Decay | Document      | Template (Train Q-A)   | Doc + Template  |
|-----------------|----------------|---------------|------------|-----------------|
llama2 13b        |  0.460         | 0.476 | **0.488** | 0.483 | 0.472|
llama2 7b         |  0.435         | **0.445** | - | - | -|
llama2_chat 13b   |  0.556         | **0.562** | 0.557 | 0.549 | 0.561 |
llama2_chat 7b    |  0.636         | **0.650** | 0.639 | - | - |

#### Pythia Full weight update (no memory module)

|model | size | 1-shot acc | method | 
|---|---|---|---|
pythia | 410m | 0.000 | gpt_template | 
pythia | 410m | 0.024 | gpt_wikipage_and_template | 
pythia | 70m | 0.000 | gpt_template | 
pythia | 70m | 0.000 | gpt_wikipage_and_template | 


---

## Reproducibility 

* **Pretrained Models**
  * `benchmark/fever/llama2_n_100.sh`
  * `benchmark/fever/pythia_n_100.sh`
* **Llama2 Memorization**:`experiments/240329_hook_mem/train_gpt_fever_llama2.sh`
* **Pythia Memorization**: `experiments/240329_hook_mem/train_gpt_fever_pythia.sh`
  <br> (we exclude Pythia with documentation memorization only.)

---

## Full Results 

### Pretrained Models

|model        | Size | Few-shot |  accuracy | 	f1.micro     |	f1.macro
|-------------|-----:|:------:|:---------:|:-------------:|:--------------:|
llama2 | 13b | 0 | 0.013 | 0.013 | 0.018 | 
llama2 | 13b | 1 | 0.460 | 0.460 | 0.319 | 
llama2 | 13b | 2 | 0.410 | 0.410 | 0.229 | 
llama2 | 13b | 3 | 0.417 | 0.417 | 0.238 | 
llama2 | 13b | 4 | 0.472 | 0.472 | 0.268 | 
llama2 | 13b | 5 | 0.476 | 0.476 | 0.272 | 
-------|-------|-------|-------|-------|-------|
llama2 | 7b | 0 | 0.018 | 0.018 | 0.024 | 
llama2 | 7b | 1 | 0.435 | 0.435 | 0.241 | 
llama2 | 7b | 2 | 0.354 | 0.354 | 0.214 | 
llama2 | 7b | 3 | 0.368 | 0.368 | 0.220 | 
llama2 | 7b | 4 | 0.367 | 0.367 | 0.219 | 
llama2 | 7b | 5 | 0.364 | 0.364 | 0.220 | 
-------|-------|-------|-------|-------|-------|
llama2_chat | 13b | 0 | 0.010 | 0.010 | 0.012 | 
llama2_chat | 13b | 1 | 0.556 | 0.556 | 0.381 | 
llama2_chat | 13b | 2 | 0.389 | 0.389 | 0.209 | 
llama2_chat | 13b | 3 | 0.496 | 0.496 | 0.259 | 
llama2_chat | 13b | 4 | 0.552 | 0.552 | 0.284 | 
llama2_chat | 13b | 5 | 0.578 | 0.578 | 0.291 | 
-------|-------|-------|-------|-------|-------|
llama2_chat | 7b | 0 | 0.392 | 0.392 | 0.196 | 
llama2_chat | 7b | 1 | 0.636 | 0.636 | 0.457 | 
llama2_chat | 7b | 2 | 0.519 | 0.519 | 0.402 | 
llama2_chat | 7b | 3 | 0.463 | 0.463 | 0.373 | 
llama2_chat | 7b | 4 | 0.408 | 0.408 | 0.341 | 
llama2_chat | 7b | 5 | 0.393 | 0.393 | 0.337 | 
-------|-------|-------|-------|-------|-------|
pythia | 12b | 0 | 0.000 | 0.000 | 0.000 | 
pythia | 12b | 1 | 0.366 | 0.366 | 0.197 | 
pythia | 12b | 2 | 0.118 | 0.118 | 0.124 | 
pythia | 12b | 3 | 0.119 | 0.119 | 0.117 | 
pythia | 12b | 4 | 0.113 | 0.113 | 0.115 | 
pythia | 12b | 5 | 0.083 | 0.083 | 0.093 | 
-------|-------|-------|-------|-------|-------|
pythia | 6.9b | 0 | 0.021 | 0.021 | 0.028 | 
pythia | 6.9b | 1 | 0.163 | 0.163 | 0.118 | 
pythia | 6.9b | 2 | 0.280 | 0.280 | 0.247 | 
pythia | 6.9b | 3 | 0.175 | 0.175 | 0.164 | 
pythia | 6.9b | 4 | 0.106 | 0.106 | 0.106 | 
pythia | 6.9b | 5 | 0.206 | 0.206 | 0.167 | 
-------|-------|-------|-------|-------|-------|
pythia | 2.8b | 0 | 0.006 | 0.006 | 0.008 | 
pythia | 2.8b | 1 | 0.026 | 0.026 | 0.025 | 
pythia | 2.8b | 2 | 0.097 | 0.097 | 0.077 | 
pythia | 2.8b | 3 | 0.056 | 0.056 | 0.066 | 
pythia | 2.8b | 4 | 0.108 | 0.108 | 0.087 | 
pythia | 2.8b | 5 | 0.132 | 0.132 | 0.102 | 
-------|-------|-------|-------|-------|-------|
pythia | 1.4b | 0 | 0.059 | 0.059 | 0.067 | 
pythia | 1.4b | 1 | 0.009 | 0.009 | 0.008 | 
pythia | 1.4b | 2 | 0.139 | 0.139 | 0.102 | 
pythia | 1.4b | 3 | 0.177 | 0.177 | 0.133 | 
pythia | 1.4b | 4 | 0.275 | 0.275 | 0.178 | 
pythia | 1.4b | 5 | 0.176 | 0.176 | 0.176 | 
-------|-------|-------|-------|-------|-------|
pythia | 1b | 0 | 0.000 | 0.000 | 0.000 | 
pythia | 1b | 1 | 0.000 | 0.000 | 0.000 | 
pythia | 1b | 2 | 0.001 | 0.001 | 0.001 | 
pythia | 1b | 3 | 0.002 | 0.002 | 0.002 | 
pythia | 1b | 4 | 0.011 | 0.011 | 0.011 | 
pythia | 1b | 5 | 0.013 | 0.013 | 0.014 | 
-------|-------|-------|-------|-------|-------|
pythia | 410m | 0 | 0.000 | 0.000 | 0.000 | 
pythia | 410m | 1 | 0.010 | 0.010 | 0.009 | 
pythia | 410m | 2 | 0.000 | 0.000 | 0.000 | 
pythia | 410m | 3 | 0.000 | 0.000 | 0.000 | 
pythia | 410m | 4 | 0.000 | 0.000 | 0.000 | 
pythia | 410m | 5 | 0.000 | 0.000 | 0.000 | 
-------|-------|-------|-------|-------|-------|
pythia | 160m | 0 | 0.000 | 0.000 | 0.000 | 
pythia | 160m | 1 | 0.000 | 0.000 | 0.000 | 
pythia | 160m | 2 | 0.000 | 0.000 | 0.000 | 
pythia | 160m | 3 | 0.000 | 0.000 | 0.000 | 
pythia | 160m | 4 | 0.000 | 0.000 | 0.000 | 
pythia | 160m | 5 | 0.000 | 0.000 | 0.000 | 
-------|-------|-------|-------|-------|-------|
pythia | 70m | 0 | 0.000 | 0.000 | 0.000 | 
pythia | 70m | 1 | 0.000 | 0.000 | 0.000 | 
pythia | 70m | 2 | 0.000 | 0.000 | 0.000 | 
pythia | 70m | 3 | 0.000 | 0.000 | 0.000 | 
pythia | 70m | 4 | 0.000 | 0.000 | 0.000 | 
pythia | 70m | 5 | 0.000 | 0.000 | 0.000 | 


### Memorization

We use one-shot for training and evaluation

llama2_chat | 13b | 0.562 | gpt_template | 
llama2_chat | 13b | 0.561 | gpt_wikipage | 
llama2_chat | 13b | 0.558 | gpt_wikipage_and_template |
llama2_chat | 7b | 0.647 | gpt_template | 
llama2_chat | 7b | 0.650 | gpt_wikipage | 
llama2_chat | 7b | 0.646 | gpt_wikipage_and_template | 
llama2 | 13b | 0.465 | gpt_template | 
llama2 | 13b | 0.476 | gpt_wikipage | 
llama2 | 13b | 0.462 | gpt_wikipage_and_template | 
llama2 | 7b | 0.445 | gpt_template | 
llama2 | 7b | 0.439 | gpt_wikipage | 
llama2 | 7b | 0.438 | gpt_wikipage_and_template | 
pythia | 12b | 0.366 | gpt_template | 
pythia | 12b | 0.366 | gpt_wikipage | 
pythia | 12b | 0.366 | gpt_wikipage_and_template | 
pythia | 6.9b | 0.163 | gpt_template | 
pythia | 6.9b | 0.163 | gpt_wikipage | 
pythia | 6.9b | 0.163 | gpt_wikipage_and_template | 
pythia | 2.8b | 0.026 | gpt_template | 
pythia | 2.8b | 0.026 | gpt_wikipage | 
pythia | 2.8b | 0.026 | gpt_wikipage_and_template | 
pythia | 1.4b | 0.009 | gpt_template | 
pythia | 1.4b | 0.009 | gpt_wikipage | 
pythia | 1.4b | 0.009 | gpt_wikipage_and_template | 
pythia | 1b | 0.000 | gpt_template | 
pythia | 1b | 0.000 | gpt_wikipage | 
pythia | 1b | 0.000 | gpt_wikipage_and_template | 
pythia | 160m | 0.000 | gpt_template | 
pythia | 160m | 0.000 | gpt_wikipage | 
pythia | 160m | 0.000 | gpt_wikipage_and_template | 
pythia | 410m | 0.010 | gpt_template | 
pythia | 410m | 0.010 | gpt_wikipage | 
pythia | 410m | 0.010 | gpt_wikipage_and_template | 
pythia | 70m | 0.000 | gpt_template | 
pythia | 70m | 0.000 | gpt_wikipage | 
pythia | 70m | 0.000 | gpt_wikipage_and_template | 