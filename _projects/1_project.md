---
layout: page
title: MBTI Prediction WAS
description: a project with a background image
img: assets/img/12.jpg
importance: 1
category: work
---


## Architecture 


### React 

- [ ] Better page constructiion : in design  

### Flask (Python Server)

- [ ] Python module must be added. 
- [ ] 

## Model 


// world similarity based model

### 1. Word Count based method 

```python
class Model():
    def __init__(self):
        # weight : 
            # if word in main_description : 1.0
            # elif word in similar word : 1.0 -  rank / total 
        self.vocab_to_score = {
            "쌀쌀" [0,1,0,1,1,1,0.5, ..., 0], 
            "규칙" [0,0,1,0.3,0.6,1,0.5, ..., 0] 
        }  

    def predict(self, sentence_list, method="weighting_word"):
        """
        given a list of words 
        return probability for 16 MBTIs
        """ 

    def weighting_word_prediction(self, sentence_list):
        ...
```
