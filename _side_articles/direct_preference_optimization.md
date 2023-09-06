---
layout: distill
title: 'Try to understand  direct preference optimization'
date: 2023-09-04
description: Try to understand  direct preference optimization (DPO)
img: 'https://drive.google.com/uc?export=view&id=17_lVnUT031VFvMMQLhrkEY6lyShWAOoQ'
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: false

---

This post represents a summary of work DPO done by Rafael et al.

## Summary

Previously, reinforcement learning (RL) is applied to human feedback (HF) fine-tuning of GPT. In RL, a **reward model** predicts the preference scores of a generated sentence and the language models (LMs) such as GPT are optimized to maximize the rewards with RL algorithms like PPO. 

The previous framework **requires a reward model** which is trained with paired sentences $y_{win}$ and $y_{lose}$.  The authors in this work conjectured that the reward model may not be required as we can **directly optimize with preference differences**. 
The proposed method which is termed as direct preference optimization (DPO) <d-cite key="rafailov2023direct"/> generated better sentences with faster convergence speed. 

> That is, the previous HF methods train a reward model which returns a preference score (scalar), but the proposed method, DPO can not require the reward model.  

---

### Overview 

**Summary** : Comparison between previous method (RLHF) and DPO which does not require the reward model. DPO  maximizes the likelihood of $y_{win}$ and minimizes the likelihood of $y_{lose}$. 

$$ 

\mathcal{L}_\mathrm{DPO}(\pi_\theta ; \pi_\mathrm{ref}) = 
- \mathbb{E}_{(x,y_w, y_l)\sim \mathcal{D}} \Big[ 
    \log \sigma 
        \Big( 
            \beta \log \frac{\pi_\theta (y_w|x)}{\pi_\mathrm{ref}(y_w | x)}
            -
            \beta \log \frac{\pi_\theta (y_l|x)}{\pi_\mathrm{ref}(y_l | x)}
        \Big)
    \Big]
$$  




<img src="https://drive.google.com/uc?export=view&id=17_lVnUT031VFvMMQLhrkEY6lyShWAOoQ" style="width:150%;margin-left:-5rem;" onClick="window.open(this.src)" >

---

### Preference Tuning Results 

**Summary** : DPO generates more positive reviews and shows better summarization results.  See <span style="color:#FFFF00;background-color:#000000">Yellow </span>. 

<img src="https://drive.google.com/uc?export=view&id=1QsfKdSPXM7u50h-0lcFT6NF1RRVGdJdI" style="width:150%;margin-left:-5rem;" onClick="window.open(this.src)" >

---

### GPT4 Win Rate

**Summary** : DPO generates more **Helpful and Harmless (HH)** dialog compared to dialogs in test sets. 


<img src="https://drive.google.com/uc?export=view&id=16EqWM8d2QEgne5yinSMIFmy4ECArmlM6" style="width:150%;margin-left:-5rem;" onClick="window.open(this.src)" >


---


### DPO Loss 

$$ 

\Large{
\mathcal{L}_\mathrm{DPO}(\pi_\theta ; \pi_\mathrm{ref}) = 
- \mathbb{E}_{(x,y_w, y_l)\sim \mathcal{D}} \Big[ 
    \log \sigma 
        \Big( 
            \beta \log \frac{\pi_\theta (y_w|x)}{\pi_\mathrm{ref}(y_w | x)}
            -
            \beta \log \frac{\pi_\theta (y_l|x)}{\pi_\mathrm{ref}(y_l | x)}
        \Big)
    \Big]
}
$$  


<d-code block language="python">

# https://github.com/eric-mitchell/direct-preference-optimization/blob/main/trainers.py
def dpo_loss(policy_chosen_logps: torch.FloatTensor,
             policy_rejected_logps: torch.FloatTensor,
             reference_chosen_logps: torch.FloatTensor,
             reference_rejected_logps: torch.FloatTensor,
             beta: float,
             reference_free: bool = False) -> Tuple[torch.FloatTensor, torch.FloatTensor, torch.FloatTensor]:
    """Compute the DPO loss for a batch of policy and reference model log probabilities.
    
    Args:
        policy_chosen_logps: Log probabilities of the policy model for the chosen responses. Shape: (batch_size,)
        policy_rejected_logps: Log probabilities of the policy model for the rejected responses. Shape: (batch_size,)
        reference_chosen_logps: Log probabilities of the reference model for the chosen responses. Shape: (batch_size,)
        reference_rejected_logps: Log probabilities of the reference model for the rejected responses. Shape: (batch_size,)
        beta: Temperature parameter for the DPO loss, 
              typically something in the range of 0.1 to 0.5. We ignore the reference model as beta -> 0.
        reference_free: If True, we ignore the _provided_ reference model 
                        and implicitly use a reference model that assigns equal probability to all responses.

    Returns:
        A tuple of three tensors: (losses, chosen_rewards, rejected_rewards).
        The losses tensor contains the DPO loss for each example in the batch.
        The chosen_rewards and rejected_rewards tensors contain the rewards for the chosen and rejected responses, respectively.
    """
    pi_logratios = policy_chosen_logps - policy_rejected_logps
    ref_logratios = reference_chosen_logps - reference_rejected_logps

    if reference_free:
        ref_logratios = 0

    logits = pi_logratios - ref_logratios

    losses = -F.logsigmoid(beta * logits)
    chosen_rewards = beta * (policy_chosen_logps - reference_chosen_logps).detach()
    rejected_rewards = beta * (policy_rejected_logps - reference_rejected_logps).detach()

    return losses, chosen_rewards, rejected_rewards

</d-code>

---



## Appendix : Math Behinds


### 1. preference distribution 

$$ 

p^*(y_1 \succ y_2 | x ) = 
\frac{
    \exp (r^* (x,y_1))
    }
    {
        \exp(r^* (x,y_1)) +\exp(r^* (x,y_2))
    }

$$  


### 2. binary classification problem framed for BT model

$\sigma$ is a sigmoid function.

$$ 

\mathcal{L}_R(r_\phi, \mathcal{D}) 
= 
- \mathbb{E}_{(x,y_w, y_l) \sim \mathcal{D}}
[
    \log \sigma(r_\phi(x, y_w) - r_\phi(x, y_l))
]

$$  

### 3. RL fine-tuning for GPT 

$$ 

\max_{\pi_\theta} \mathbb{E}_{x\sim \mathcal{D}, y\sim \pi_\theta (y|x)}[r_\pi(x,y)] 
- \beta \mathbb{D}_\mathrm{KL}[\pi_\theta(y|x) \Vert \pi_\mathrm{ref}(y|x)]

$$  


### 4. The form of the optimal solution of Equation 3

$$ 

\pi_r(y|x) = \frac{1}{Z} \pi_\mathrm{ref}(y|x) \exp \Big( \frac{1}{\beta} r(x,y) \Big)

$$  

### 5. reward function obtained from Equation 4. 

$$ 

r(x,y) = \beta \log \frac{\pi_r(y|x)}{ \pi_\mathrm{ref}(y|x)} + \beta \log Z(x)

$$  

### 6. The preference probability expressed in terms of the policies.

$$ 

p^*(y_1 \succ y_2 | x) = \frac{1}
            {
                1+ \exp \Big( \beta \log \frac{\pi^*(y_2|x)}{\pi_\mathrm{ref}(y_2|x)} 
                - \beta \log \frac{\pi^* (y_1|x)}{\pi_\mathrm{ref}(y_1|x)} \Big)
            }

$$  

### 7. DPO maximum likelihood objective 

$$ 

\mathcal{L}_\mathrm{DPO}(\pi_\theta ; \pi_\mathrm{ref}) = 
- \mathbb{E}_{(x,y_w, y_l)\sim \mathcal{D}} \Big[ 
    \log \sigma 
        \Big( 
            \beta \log \frac{\pi_\theta (y_w|x)}{\pi_\mathrm{ref}(y_w | x)}
            -
            \beta \log \frac{\pi_\theta (y_l|x)}{\pi_\mathrm{ref}(y_l | x)}
        \Big)
    \Big]
$$  


--- 
