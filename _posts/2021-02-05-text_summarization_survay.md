---
title: 'Text Summarization 분야 탐색'
date: 2021-02-05
permalink: /posts/2021/02/text_summarization_survay/
tags:
  - summarization
  - nlp
  - literature_review
  - survey
---

- [Text Summarization](#text-summarization)
  - [Text Summarization의 목표](#text-summarization의-목표)
  - [Text Summarization의 세분화](#text-summarization의-세분화)

# Text Summarization

Text Summarization은 문서에서 중요한 정보만을 선정하여 Summary를 만드는 테스크입니다. 최근 Transformer 아키텍쳐의 발명으로 Summarize의 성능은 크게 향상되었습니다. 여기서 더욱 발전하기 위해서, Text Summarization에 대하여 어떠한 연구들이 진행되었는지 알고 이를 바탕으로 중요하게 다뤄야 하는 부분을 알고자 합니다. 본 포스팅에서는 자연어처리(NLP)의 하위 테스크인 Text Summarization에 대한 기존 연구들을 정리하였습니다. 


## Text Summarization의 목표

주어진 문서에 대해서 단순하게 키워드를 뽑아내는 Topic Modeling과는 다르게 Text Summarization은 Text Generation의 하위 테스크입니다. Summary를 사람이 한 것처럼 만들기 위해서는 문서에 대한 요약이 필요하며 Summary 자체의 특성을 알아야 합니다.

특징들은 다음과 같습니다. [1]

1. 간결하고 논리 정연하다.
2. 유창하고 일관성 있다.
3. 모든 중요한 화제(Topic)을 잡아낸다. 
4. 동일한 정보에 대해서 반복하지 않아야 한다. 


## Text Summarization의 세분화

Text Summarization은 더욱 세분화가 될 수 있습니다. 

**1.** Human vs Machine 

  Human Summarization은 사람이 직접 Summarization을 하는 것 입니다. 모델에게 학습 시키고자 하는 바 입니다. Machine Summarization은 Automatic Summarization이라고 불리며, 우리가 학습 시킨 모델로 Summary를 만드는 것 입니다. 

**2.** Extractive vs Abstractive

  Summarization을 하면 가장 많이 나오는 구분입니다. 문서로부터 주요 정보를 추출하는 목적을 가지고 있기 때문에, 문서에 있는 중요한 문장을 선택할 수 있습니다. Extractive는 문서의 내용을 변경하지 않고 주요 문장을 추출하는 것 입니다. 이와는 다르게 Abstractive Summarization은 문서에 있던 문장들을 그대로 사용하지 않고 Paraphrase해서 요약하는 방법입니다. Extractive Summarization의 문제점은 문장을 선택함으로써 전체 Summary가 부자연스러울 수 있고, 내용이 의미하는 바를 모델이 알지 못할 가능성이 높다는 것 입니다. 이는 문장을 paraphrase하지 못하기 때문입니다. 따라서 최근에는 Extractive에서 Abstractive로 Text Summarization의 흐름이 바뀌었습니다. 

|![images](/images/posts/text_summarization_survey/img1.png)|
|---|
|이 문장은 두 문장의 합으로 이루어져 있습니다. Abstractive Summarization으로 두 문장이 지닌 의미를 축약할 수 있습니다.|

**3.** Single vs Multi Document Summarization

Single Document Summarization은 주어진 1개의 문서에 대해서 요약을 만들고, Multi는 여러 개의 문서가 주어졌을 때, Summary를 만드는 Task입니다. 물론 Multi Document Summarization에서 문서를 모두 붙여서 하나의 Single Document를 만들고 Summary를 만들면 됩니다. 물론 거기까지 가기 전에, 단순하게 문서들을 붙이는 방법말고도 여러가지 기술을 적용할 수 있기 때문에 구분이 됩니다. 최근 Deeplearning 관련된 연구들은 대부분 Single Summary의 성능을 올리는데 초점이 맞춰져 있습니다.     

**4.** Query-based Summarization

Query-based는 Summarization은 Multdocument에서 주어진 Query(질문)에 대해서 관련된 문서의 부분을 요약하는 Task입니다. query에 해당하는 정보를 찾아서 요약해주기 때문에, extractive와 abstractive 모두 사용이 가능합니다. 

![images](/images/posts/text_summarization_survey/img2.png)


**4.** Update Summarization

사용자가 이전 정보를 알고 있다는 가정하에, 새로운 정보에 대해서 요약을 하는 것 입니다. 기존에 사용자가 알고 있는 정보를 제외하고 추가적인 정보를 제공해야 한다는 제약이 있습니다. 위에서 이야기한  ```동일한 정보에 대해서 반복하지 않아야 한다.```는 개념과 비슷합니다. 

**5.** Personalization

Summary를 하나의 방식으로 요약하는 것을 넘어서, 독자를 고려한 Summarization입니다. 의학이나 과학저널 같은 전문적인 분야에 좀더 적합합니다. 이 포스팅 또한 읽고 있는 사람에 따라서 summarization이 다를 수 있습니다. 


---
추가예정
