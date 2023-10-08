---
layout: share-distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: false
disqus_comments: false
date: 2023-10-04
featured: true
img: 
title: '[🧪 EXP Note] <br> Source Identification Problem <br> GPT last hidden representation probing '
description: 'GPT 모델 파라미터로 문장을 인코딩하여, 다음 단어 예측 표현으로 원천소스를 맵핑한다. '
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

## 1. Introduction 

데이터에 대한 원천소스를 찾기 위해서 할 수 있는 쉬운 방법은 텍스트를 암기하는 것이다. 주어진 모델에 대해서 암기를 하는 방법은 크게 두 가지가 있다. 
1) 주어진 문장의 N-gram에 대해서 원천소스에 맵핑하는 방법 2) 주어진 문장을 모델에 넣고 주어진 표현으로 원천소스를 맵핑하는 방법. 이 중 두 번째, 주어진 문잦을 모델에 넣는 방법은 주어진 문장에 대한 모델의 표현을 얻어, 이로부터 원천소스를 맵핑하는 방법이다. 만일 모델이 주어진 문장에 대해서, 다음에 나타날 단어를 어느정도 알고 있다면, 그 정보로부터 문장의 출처를 암기하는데 사용할 수 있다는 관점이다. 

1. 모델이 문장에 대해서 다음에 나타날 표현을 알고 있다. 
2. 알고 있는 문장이므로, 소스를 암기해야 한다. 

반대로, 문장의 표현을 제대로 알고 있지 못하다면, 다음에 나타나는 단어들은 랜덤한 형태를 띄고, 원천소스를 암기할 필요가 없다. 
즉, **문장의 의미를 알고 있다면, 원천소스도 기억해라** 라는 의무적인 모델링 방식이다. 

이러한 철학으로부터 다음으로 고민할 문제는 문장의 의미를 인지하고 있는지 알고 있는 표현으로부터 원천소스를 맵핑하는 방법이다. 
이는 Probing 연구와 비슷하게, 해당 표현으로부터 원천소스를 맵핑하는 Classification 혹은 Generation 문제로 치환하는 것이다. 
이 실험에서는 간단한 Probing + Classification 으로 두 가지를 확인한다. 

1. 문장이 길수록 원천소스 암기력은 올라가는가?
2. 모델 사이즈가 클수록 암기력은 올라가는가? 

두 가지는 새로운 내용이 아니며, 어느정도 당연한 사실이다. 이 사실을 실험을 통해서 검증한다. 
Probing에 사용된 방식은 GPT 모델에 문장을 넣고 나오는 Next Token Prediction 전 Hidden Representation 을 Mean Pooling하고, LayerNorm 을 거쳐서 128차원 ReLU Activation을 가지는 6개 Linear Layer MLP 모델이다. 또한, 원천소스는 10개의 클래스에 대해서 진행하였고, 각 클래스별 샘플 수는 최소 200개에서 최대 1000개이다. 


## 2. Number of Words Comparison

* Hyperparameter is the for num_words in `[4 8 16]`
* Model Size : `Pythia-1b`

**결론** : 단어 길이가 길수록 정확도가 오르며, 너무 짧은 경우 학습 Loss 가 줄어들지 않았다. 8개부터는 보다 자연스러운 학습이 되는 것으로 간주된다. 

데이터 생성 시 원천소스의 문장들은 단어 개수로 나누기 때문에, 단어 개수가 많아질수록 학습 데이터 수는 줄어든다. 따라서 8개 정도면 wikitext2 데이터에서 충분한 샘플 수를 보장할 수 있는 것으로 보인다. 


<figure style='margin-left:-10rem; width:150%'>
<img src="https://drive.google.com/uc?export=view&id=1A-QHI96khQt84rwlSE6judHweaPby4pF">
</figure>


---


## 3. Pythia Model Size Comparison : Binary 


* Hyperparameter is the model size : `['70m', '160m', '410m', '1b', '1.4b', '2.8b', '6.9b', '12b']`
* num_words: `8`

여기서는 모델 사이즈에 대해서 검증한다. 모델 사이즈가 클수록 다음에 나타나는 단어를 정확하게 예측할 수 있으므로, 성능이 우수하다. 
`1B` 이상의 모델이 원천소스를 기억하는데 사용될 수 있으며, `6B` 이상부터는 더욱 빠르게 원천소스를 예측할 수 있는 것을 확인한다. 


<figure style='margin-left:-10rem; width:150%'>
<img src="https://drive.google.com/uc?export=view&id=1F0TvufzyBdxlGqgxQDdU7D7Q2D5sKfck">
</figure>


여기서 Class Label로 활용된 방식은 Binary 로, 아래와 같은 형태의 레이블을 지닌다. 

<d-code language='python'>
----Hash-------
 Valkyria Chronicles III                   : 0001
 USS Atlanta ( 1861 )                      : 0010
 Gambia women 's national football team    : 0011
  Mary Barker                              : 0100
 2011 – 12 Columbus Blue Jackets season    : 0101
 There 's Got to Be a Way                  : 0110
 Plain maskray                             : 0111
 Gregorian Tower                           : 1000
 Nebraska Highway 88                       : 1001
  ; Sv %                                   : 1010
 Tower Building of the Little Rock Arsenal : 1011
---------------
</d-code>



---


## 4. Pythia Model Size Comparison : Category 

* Hyperparameter is the model size : `['70m', '160m', '410m', '1b', '1.4b', '2.8b', '6.9b', '12b']`
* num_words: `8`

단순 원천소스 분류의 경우, 레이블을 Binary형태가 아닌 Category 형태로 만들 수 있다. 이 경우, `410m` 모델 또한 학습이 가능했다. 
그러나 여전히 모델 사이즈가 커야 학습이 된다. `70m`의 경우 학습이 되지 않는 것을 확인할 수 있다. 

궁극적으로, 모델 사이즈가 작은 경우, Category 방식이 Binary 방식보다 좀더 학습이 잘 되는 것을 확인하였는데, 
모델 사이즈가 큰 경우 두 학습 결과에는 차이가 없다. 


<figure style='margin-left:-10rem; width:150%'>
<img src="https://drive.google.com/uc?export=view&id=1zra8OkZYg0dANK7RbcsmyUJ3k14dVwFI">
</figure>


<d-code language='python'>
----Hash-------
 Valkyria Chronicles III                   : 1
 USS Atlanta ( 1861 )                      : 2
 Gambia women 's national football team    : 3
  Mary Barker                              : 4
 2011 – 12 Columbus Blue Jackets season    : 5
 There 's Got to Be a Way                  : 6
 Plain maskray                             : 7
 Gregorian Tower                           : 8
 Nebraska Highway 88                       : 9
  ; Sv %                                   : 10
 Tower Building of the Little Rock Arsenal : 11
---------------
</d-code>



## 5. 결론 및 추가 실험 

다음 단어 예측으로 원천소스를 인코딩 하는 경우, 모델이 문장을 암기하는 수준으로부터 원천소스 암기 가능성을 볼 수 있다. 
이는 Memorization을 평가하는 한 가지 방법으로 보인다. 

원천 소스 정확도가 목적인 경우 대안은 다음과 같다. 적어도 성능 비교를 위해서 할 필요는 있다. 다만, 해당 테스크는 원천소스와 모델 파라미터의 관계에 대해서 정보를 주지 못하므로, 본 연구와는 차이가 있다. 

1. 개별 모델 BERT Encoding --> Prediction (단점: 모델과 원천소스의 관계성은 없다. 원천소스 예측 Task)
2. GPT Input Embedding 사용 (Prediction 대신)

추가 데이터셋 
1. Plain English : 책 어휘와 같은 데이터 셋 
2. PhilPaper : 논문 데이터셋



--- 

<button onclick="myFunction(1)" style="background-color:#FFDD00;border-radius:10px">Open Appendix</button>

<div id="1" style="display:none;border:3px solid #DDDDDD;padding:1rem;" markdown="1">

# Appendix 

## Code Release

* **Github Code Release**
    * Repository: source_identification_problem 
    * Version [v23.10.04.2](https://github.com/fxnnxc/source_identification_problem/tree/v23.10.04.2)
* **W&B** :
    * binary : https://wandb.ai/bumjin/sip-pythia_comparison_binary?workspace=user-bumjin
    * category : https://wandb.ai/bumjin/sip-pythia_comparison_category?workspace=user-bumjin
    * number of words : https://wandb.ai/bumjin/sip-num_words_comparison?workspace=user-bumjin



## Word Examples 

랜덤하게 학습데이터에서 추출한 데이터 

|Label (Source) | Text Chunk (Length 4) |
|---| ---|
 USS Atlanta ( 1861 )   | the 15 @-@ inch                                   
 USS Atlanta ( 1861 )   | opened fire with both                             
 Tower Building of the Little Rock Arsenal   | Guard , office ,                                  
 Valkyria Chronicles III   | follows the " Nameless                            
 2011 – 12 Columbus Blue Jackets season   | After the game ,                                  
 2011 – 12 Columbus Blue Jackets season   | soon to be free                                   
 2011 – 12 Columbus Blue Jackets season   | his second game ,                                 
  Mary Barker   | remarkable freedom of spirit                      
 Tower Building of the Little Rock Arsenal   | last week in the                                  
  Mary Barker   | deteriorate . She was        



|Label (Source) | Text Chunk (Length 8) |
|---| ---|
 USS Atlanta ( 1861 )   | ( 381 mm ) shell struck the ironclad              
 Valkyria Chronicles III   | , experience points are awarded to the squad      
 Valkyria Chronicles III   | other is sealed off to the player .               
 Tower Building of the Little Rock Arsenal   | last part of the month . " The                    
 Tower Building of the Little Rock Arsenal   | . The arsenal was briefly seized once more        
 John Cullen   | with 110 points combined between the Penguins and 
 2011 – 12 Columbus Blue Jackets season   | their first three @-@ game win streak of          
 Valkyria Chronicles III   | with Valkyria Chronicles II director Takeshi Ozawa .
  Mary Barker   | with an analytical eye and was friend to          
 Tower Building of the Little Rock Arsenal   | , and machinery at the Little Rock Arsenal 



|Label (Source) | Text Chunk (Length 16) |
|---| ---|
 2011 – 12 Columbus Blue Jackets season   | the third worst point total in franchise history . = = Off @-@ season = =
 General aviation in the United Kingdom   | more specific terminology can be used to <unk> its purpose . The CAA strategic review of
 Ancient Egyptian deities   | . Many gods had more than one cult center , and their local ties changed over
 John Cullen   | with 110 points combined between the Penguins and Whalers , and was the team 's best
 Jacqueline Fernandez   | balloon " . Later that year , she made a cameo appearance in Sajid Khan 's
 South of Heaven   | Russell Simmons and Rubin parted ways , Slayer signed to Rubin 's newly founded Def American
 Ancient Egyptian deities   | god 's connections and interactions with other deities helped define its character . Thus Isis ,
 General aviation in the United Kingdom   | Cessna and Piper introduced light aircraft designed for the private market . The Cessna 172 ,
 SMS Zrínyi   | Saint @-@ Germain @-@ en @-@ <unk> , the transfer was not recognized ; instead ,
 Jacqueline Fernandez   | , a role based on the Princess Jasmine character . Fernandez garnered mixed reviews for her


## Training Settings 

Memorization 에 사용된 모델은 다음과 같다. 
* linear_hidden_size: 256
* linear_activation: relu
* linear_n_layers: 6


<d-code language='python'>
👾 FLAGS
datetime: '10_03_203434'
data: wikitext-2-v1
data_cache_dir: /data/EleutherAI_wikitext
num_words: 8
max_tokens: 32
lm_batch_size: 8
num_first_labels: 10
num_samples_per_label: 1000
min_samples_per_label: 200
datasets_split_ratio: 0.8
exp_name: pythia_comparison_binary
test: false
lm_model: pythia
lm_model_size: 1b
lm_model_path: /data/EleutherAI
lm_num_gpus: 2
lr: 0.001
batch_size: 4
hash_type: binary
device: cuda:0
num_warmup_steps: 1000
num_epochs: 1000
num_train_steps: 33000
num_eval: 10
num_save: 10
hash_model_type: linear
hash_batch_size: 128
linear_hidden_size: 256
linear_activation: relu
linear_n_layers: 6
rnn_hidden_size: 128
rnn_n_layers: 2
bert_num_hidden_layers: 12
bert_replace_to_bert: false
task: train_hash
project_name: sip-pythia_comparison_binary
num_binaries: 4
num_labels: 12
hidden_size: 2048
num_steps_per_epoch: 33
save_freq: 3300
eval_freq: 3300
global_step: 0
</d-code>

</div>






<script>
function myFunction(n) {
  var x = document.getElementById(n);
  if (x.style.display === "none") {
    x.style.display = "block";
  } else {
    x.style.display = "none";
  }
}
</script>