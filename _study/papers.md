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

---

### Template


* Authors : 
* Published Date : 
* Record Date : 

<button onclick="myFunction(10000)" style="background-color:#FFFFDD;border-radius:10px">Details</button>

<div id="10000" style="display:none;border:3px solid #DDDDDD;padding:1rem;" markdown="1">

<button onclick="myFunction(10000)" style="background-color:#DDDDDD;border-radius:10px">Close Details</button>
</div>

---

### Watermarking Text Data on Large Language Models for Dataset Copyright Protection


* Authors : Yixin Liu, Hongsheng Hu, Xuyun Zhang, Lichao Sun
* Published Date : 2023.05.22, Arxiv 
* Record Date : 

<button onclick="myFunction(8)" style="background-color:#FFFFDD;border-radius:10px">Details</button>

<div id="8" style="display:none;border:3px solid #DDDDDD;padding:1rem;" markdown="1">

<button onclick="myFunction(8)" style="background-color:#DDDDDD;border-radius:10px">Close Details</button>
</div>

---



### Robust Multi-bit Natural Language Watermarking through Invariant Features 


* Authors : KiYoon Yoo, Wonhyuk Ahn, Jiho Jang, Nojun Kwak
* Published Date : ACL 2023 
* Record Date : 2023.09.15

<button onclick="myFunction(7)" style="background-color:#FFFFDD;border-radius:10px">Details</button>

<div id="7" style="display:none;border:3px solid #DDDDDD;padding:1rem;" markdown="1">

This calls for a secure watermarking system to guarantee copyright protection through leakage tracing or ownership identification. 

Our full method improves upon the previous work on robustness by +16.8% point on average on four datasets, three corruption types, and two corruption ratios. 

Digital watermarking is a technology that enables the embedding of information into multimedia in an unnoticeable way without degrading the original utility of the content. 

Deep watermarking has emerged as a new paradigm that improves the three key aspects of watermarking: payload (i.e., the number of bits embedded), robustness (i.e., the number of bits embedded), robustness (i.e., accuracy of the extracted message), and quality of embedded media. 

Previous research has focused on techniques such as lexical substitution with predefined rules and dictionaries or structural transformation. 

Recent work have either replaced the predefined set of rules with learning-based methodology, thereby removing heuristics or vastly improved the quality of lexical substitutions. 

A well-known proposition of a classical image watermarking work: That watermarks should *"be placed explicitly in the perceptually most significant components"*  of an image. If this is achieved, the adversary must corrupt the content's fundamental structure to destroy the watermark. This degrades the utility of the original content, rendering the purpose of pirating futile. 

Modification in individual pixels is much more imperceptible than on individual words. But to this, while we adhere to the gist of the proposition, we do not embed directly on the most significant component. Instead, we identify features that are semantically or syntactically fundamental components of the text and thus, invariant to minor modifications in texts. 



<button onclick="myFunction(7)" style="background-color:#DDDDDD;border-radius:10px">Close Details</button>
</div>


---


### CodeIPPrompt: Intellectual Property Infringement Assessment of Code Language Models 

* Authors : Zhiyuan Yu, Yuhao Wu, Ning Zhang, Chenguang Wang, Yevgeny Vorobeychik, Chaowei Xiao. 
* Published Date : 
* Record Date : 

<button onclick="myFunction(6)" style="background-color:#FFFFDD;border-radius:10px">Details</button>

<div id="6" style="display:none;border:3px solid #DDDDDD;padding:1rem;" markdown="1">

<button onclick="myFunction(6)" style="background-color:#DDDDDD;border-radius:10px">Close Details</button>
</div>



---


### A Watermark for Large Language Models 

* Authors : John Kirchenbauer, Jonas Geiping, Yuxin Wen, Jonathan Katz, Ian Miers, Tom Goldstein 
* Published Date : 2023 ICML (Best Paper Award)
* Record Date : 2023.09.18

<button onclick="myFunction(5)" style="background-color:#FFFFDD;border-radius:10px">Details</button>
<div id="5" style="display:none;border:3px solid #DDDDDD;padding:1rem;" markdown="1">








<button onclick="myFunction(5)" style="background-color:#DDDDDD;border-radius:10px">Close Details</button>
</div>


---

<span class="spanbox"> Metric:Repetition </span>  <span class="spanbox"> Rep-n </span>

### Neural Text Generation with Unlikelihood Training

* Authors : Sean Welleck, Ilia Kulikov, Stephen Roller, Emily Dinan, Kyunghyun Cho, Jason Weston
* Published Date : 2019.08
* Record Date : 2023.09.12 

<button onclick="myFunction(4)" style="background-color:#FFFFDD;border-radius:10px">Details</button>

<div id="4" style="display:none;border:3px solid #DDDDDD;padding:1rem;" markdown="1">

sequence-level repetition as the portion of duplicate n-grams in the generated text.  For a generation text $x$, Rep-n can be formulated as: 

$$

\operatorname{Rep}_{\operatorname{n}} = 100 \times \Big( 1.0 - \frac{ \operatorname{unique n gram}(x)}{\operatorname{total n gram}(x)} \Big)

$$

MAUVE: Measuring the Gap Between Neural Text and Human Text using Divergence Frontiers

MAUVE, ( Pillutla et al., 2021)  + Factor 2.0 used in Su et al., 2022, Krishna et al., 2022) 


<button onclick="myFunction(4)" style="background-color:#DDDDDD;border-radius:10px">Close Details</button>
</div>



---

<span class="spanbox"> Copyright </span>


### Evading Water mark based Detection of AI-Generated Content

* Authors : Zhengyuan Jiang, Jinghuai Zhang, Neil Zhenqiang Gong 
* Published Date : 2023.08.22  (Arvix) / ACM Conference on Computer and Communications Security (CCS), 2023 <d-cite key="jiang2023evading"/>
* Record Date : 2023.09.12 

<button onclick="myFunction(3)" style="background-color:#FFFFDD;border-radius:10px">Details</button>

<div id="3" style="display:none;border:3px solid #DDDDDD;padding:1rem;" markdown="1">



The watermark enables proactive detection of Ai-generated content in the future: a content is AI-generated if a similar watermark can be extracted from it. 

* DALL-E  : visible watermark at the bottom right corner of its generated images.
* Stable Diffusion  : non-learning-based watermarking method 
* Meta : learning-based watermarking methods 

---

* image, watermark (bitstring)
* encoder : given an image and a watermark, an encoder embeds the watermark into the image to produce a *watermarked image*
* decoder : given a *watermarked image* generates the watermark inside of it. 

An image is predicted as AI-generated if the bitwise accuracy of the decoded watermark is larger than a threshold $\tau$, where bitwise accuracy is the fraction of matched bits in the decoded watermark and the ground-truth one.
The threshold should be larger than 0.5 since the bitwise accuracy of original images without watermarks would be around 0.5. 

Robustness against *post-processing*, which post-processes an AI-generated image, is crucial for a watermark-based detector. 


See [ML-False Positive Rate](/side_articles/ml/#false-positive-rate)


<button onclick="myFunction(3)" style="background-color:#DDDDDD;border-radius:10px">Close Details</button>
</div>





---

<span class="spanbox"> Copyright </span>

### Confidence-Building Measures for Artificial Intelligence: <br> Workshop Proceedings 


* Authors : Sara Shoker, Andrew Reddie, et al., 
* Published Date : 2023.08.03 (Arxiv) <d-cite key="shoker2023confidencebuilding"/>
* Record Date : 2023.09.12 

<button onclick="myFunction(2)" style="background-color:#FFFFDD;border-radius:10px">Details</button>

<div id="2" style="display:none;border:3px solid #DDDDDD;padding:1rem;" markdown="1">


Provenance and watermarking methods can improve **traceability**, alleviate concerns about **the origin of the AI generated or edited content**, and promote trust among parties. 

If properly vetted against adversarial manipulation, they can also help states use AI-generated products more confidently, knowing that **the outcomes can be traced back to their source**. 


Coalition for Content Provenance and Authenticity (C2PA), whose members include Adobe, Microsoft, Intel, and so on, is an industry-led initiative that develops technical standards for establishing **the source and history of media content**.

C2PA specifications, provenance methods can be split between "hard" and "soft" binding

* **SOFT** : Watermarking (they are more easily undermined with modification of contents)
* **HARD** : methods for applying unique identifiers to data assets and other cryptographic methods. (using cryptographically-bound provenance can include information about the origin of a piece of content such as AI model or version used to create it. )

More Info on *[PAI's Responsible Practices for Synthetic Media](https://syntheticmedia.partnershiponai.org/#learn_more)* 

Watermarking can serve as a verification mechanism to confirm the authenticity and integrity of AI generations. 
Watermarking involves embedding low probability sequences of tokens into the outputs produced by AI systems. 

* **Drawbacks** : Watermarks are not <span class="spanbox" style='background-color:#FFEEEE;'> tamper-proof </span> (변조 방지). Bad actors can use "paraphrasing attacks" to **(1) remove text watermarks**, **(2) spoofing to infer hidden watermark signatures**, or even **(3) add watermarks to authentic content**. 

* Removal of Watermark **Evading Watermark based Detection of AI-Generated Content**  <d-cite key="jiang2023evading"/> [[Arvix](https://arxiv.org/abs/2305.03807)].


Open provenance standards and open sourcing **AI detection technologies** should be encouraged to help reduce the cost of security. 

The proliferation of foundation models means that provenance and watermarking is unlikely to be applied evenly by all developers.


<button onclick="myFunction(2)" style="background-color:#DDDDDD;border-radius:10px">Close Details</button>
</div>

---


<span class="spanbox"> Copyright </span>

### How to Protect Copyright Data in Optimization of Large Language Models? 

* Authors : Timothy Chu, Zhao Song, Chiwun Yang
* Published Date : 2023.08.23 (Arxiv) <d-cite key="chu2023protect"/>
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