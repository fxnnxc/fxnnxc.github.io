---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2023-09-10
featured: true
title: 'Papers'
description: 'All the papers...'
importance: 2 
---

### How to Protect Copyright Data in Optimization of Large Language Models? 

* Authors : Timothy Chu, Zhao Song, Chiwun Yang
* Published Date : 2023.08.23 (Arxiv) 
* Record Date : 2023.09.11 

<button onclick="myFunction(1)" style="background-color:#FFFFDD;border-radius:10px">Details</button>


<div id="1" style="display:none;border:3px solid #DDDDDD;padding:1rem;" markdown="1">

LLMs are built on the transformer neural network architecture, which in turn relies on a mathematical computation called Attention that uses the softmax function. \

To solve copyright regression for the softmax function, we show that the objective function of the softmax copyright regression is convex, and that its Hessian is bounded. 

* [Gil 19 ] investigates copyright infringement in AI-generated artwork and argues that using copyrighted works during the training phase of AI programs does not result in infringement liability. 

* [VKB23] proposes a frameowkr that provides stronger protection against sampling protected content, by defining near access-freeness (NAF)

--- 


<blockquote>
($\tau$-Copyright-Protected )
<br>

If there is a trained model $f_\theta$ with parameter $\theta$ that satisfies 

$$
\frac{L(f_\theta(A_1), b_1)}{n_1} \ge \tau + \frac{L(f_\theta(A_2)), b_2}{n_2}
$$

then we say this model $f_\theta$ is $\tau$-Copyright-Protected. 
</blockquote>


<button onclick="myFunction(1)" style="background-color:#DDDDDD;border-radius:10px">Close Details</button>

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