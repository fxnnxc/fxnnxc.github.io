---
title: 'Inference of the Transformer is tricky!'
date: 2021-01-21
permalink: /posts/2021/01/transformer-inference/
tags:
  - training
  - transformer
  - bart
  - infernece
---

## Inference of the transformer

**Inference stage** of the transformer architecture is different with RNN case. 

When we encode a source sentence, we get a hidden state of the source. When we decode that hidden state, we should generate tokens one by one. 

Therefore we run **encoder only 1 time but decoder several times**.

Because of this property, we don't use the forward method in the transformer, but instead we use forward method in the encoder and the decoder each. 

In the fairseq code, this happens in ```Sequence Generator``` class. If you want to decode tokens with additional layers, then change the ```def generate``` code to use additional layers.  


## 1. Make additonal methods for the Model.  

```python
# additional functions in the model.py
    def get_latent_z(self, x):
        x_z, x_mu, x_lv = self.latent_x_layer(x)
        return x_z, x_mu, x_lv        
    
    def get_latent_prob(self, z):
        return self.ZtoV(z)

    def get_prob(self, x):
        return self.XtoV(x)
```

## 2. Now change the sequence generator's forward decoder. 

```python
def forward_decoder(
        # ...
        encoder_out = encoder_outs[i]
        latent_z, _, _ = model.get_latent_z(encoder_out['encoder_out'][0])
        latent_lprob = model.get_latent_prob(latent_z)

        decoder_prob = model.get_prob(decoder_out[0]) + latent_lprob
        decoder_out = (decoder_prob, decoder_out[1])
        # ...

``` 

## References 

[1] fairseq 
    https://github.com/pytorch/fairseq