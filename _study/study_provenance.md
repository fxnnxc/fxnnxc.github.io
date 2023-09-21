---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2023-09-17
featured: true
title: 'Data Provenance'
description: 'Study of Provenance.'
---


## List of Papers 

* Membership Inference Attacks on Sequence-to-Sequence Models: Is My Data In Your Machine Translation System? [🔗](#membership-inference-attacks-on-sequence-to-sequence-models-is-my-data-in-your-machine-translation-system) <d-cite key="hisamoto2020membership"/>
* Membership Inference Attacks Against Machine Learning Models [🔗](#membership-inference-attacks-against-machine-learning-models)  <d-cite key="shokri2017membership"/>
* Watermarking Text Data on Large Language Models for Dataset Copyright Protection [🔗](#watermarking-text-data-on-large-language-models-for-dataset-copyright-protection) <d-cite key="liu2023watermarking"/>
* Robust Multi-bit Natural Language Watermarking through Invariant Features [🔗](#robust-multi-bit-natural-language-watermarking-through-invariant-features) <d-cite key="yoo2023robust"/>
* CodeIPPrompt: Intellectual Property Infringement Assessment of Code Language Models  [🔗](#codeipprompt-intellectual-property-infringement-assessment-of-code-language-models) <d-cite key="yu2023codeipprompt"/>
* A Watermark for Large Language Models [🔗](#a-watermark-for-large-language-models) <d-cite key="kirchenbauer2023watermark"/>
* Evading Water mark based Detection of AI-Generated Content [🔗](#evading-water-mark-based-detection-of-ai-generated-content)  <d-cite key="jiang2023evading"/>
* How to Protect Copyright Data in Optimization of Large Language Models?  [🔗] <d-cite key="shoker2023confidence"/> (#how-to-protect-copyright-data-in-optimization-of-large-language-models) <d-cite key="chu2023protect"/>
* Confidence-Building Measures for Artificial Intelligence: [🔗](#confidence-building-measures-for-artificial-intelligence-workshop-proceedings) 
* ContextLS  <d-cite key="yang2022tracing"/>
* AWT  <d-cite key="abdelnabi2021adversarial"/>




## Short Summary 

* Paper <d-cite key="hisamoto2020membership"/> :  The authors propose MI framework on sequence-to-sequence generation and shows similar private data leakage with classification problems.
* Paper <d-cite key="shokri2017membership"/>  : The Membership Inference problem which determines whether a private data is used in the training of LMs, firstly proposed in this work. 
* Paper  <d-cite key="liu2023watermarking"/> : The backdoor attack approach, which makes a desired output with a stealthy included trigger, is applied to watermark input sequence. 
* Paper <d-cite key="yu2023codeipprompt"/> : This work shows that LM based code generators memorize the code beyond the licensed usage.
* Paper <d-cite key="kirchenbauer2023watermark"/> : This work proposed watermark framework based on the distribution of selected tokens. This work is the firts work which utilizes the watermark on the distribution of tokens in generation. 
* Paper <d-cite key="jiang2023evading"/> : T
* []

--- 

<img src="https://drive.google.com/uc?export=view&id=1DaSSF2dSi46Vgt5c2rogKTIlGEwyX8Le" style="margin-left:-7rem;width:140%">




---

### Membership Inference Attacks Against Machine Learning Models 

Membership inference: given a machine learning model and a record, determine whether this record was used as part of the model's training dataset or not. 

The adversary's access wto the model is limited to black-box queries that return the model's output oon a given input. 


> Backdoor Attack : lead a model to infer the desired output with a stealthy included trigger.  As backdoor attack can output the desired pattern, it could be used for watermarks. 

---

### Membership Inference Attacks on Sequence-to-Sequence Models: Is My Data In Your Machine Translation System? 

MI : given a machine learning model and a record, determine whether this record was used as part of the model's training dataset or not.

MI+Seq-to-Seq : Given black-box access to an MT model, is it possible to determine whether a particular sentence pair was in the training set for that model? 


Simple one-off attacks based on shadow models, which proved successful in classification problems, are not successful on sequence generation problems: this is a result that favors the defender. Nevertheless, we describe the specific conditions where sequence-to-sequence models still leak private information. 

A training set for MT consists of a set of sentence pairs $\{ (f_i^{(d)}, e_i^{(d)})\}$. We use a label $d\in \{ \ell_1, \ell_2, \cdots, \}$ to indicate the domain (the subcorpus or the data source). 


---

### Watermarking Text Data on Large Language Models for Dataset Copyright Protection


* Authors : Yixin Liu, Hongsheng Hu, Xuyun Zhang, Lichao Sun
* Published Date : 2023.05.22, Arxiv 
* Record Date : 


Specifically, there is a potential risk to individual's privacy if their private data is used to train these LLMs without any authorization. This has incited significant apprehension regarding both data privacy and copyright protection. 


To prevent the illegal or unauthorized usage of private text data in LLMs, individuals should be able to verify whether or not a company used their personal text to train a model.


---



### Robust Multi-bit Natural Language Watermarking through Invariant Features 


* Authors : KiYoon Yoo, Wonhyuk Ahn, Jiho Jang, Nojun Kwak
* Published Date : ACL 2023 
* Record Date : 2023.09.15

This calls for a secure watermarking system to guarantee copyright protection through leakage tracing or ownership identification. 

Our full method improves upon the previous work on robustness by +16.8% point on average on four datasets, three corruption types, and two corruption ratios. 

Digital watermarking is a technology that enables the embedding of information into multimedia in an unnoticeable way without degrading the original utility of the content. 

Deep watermarking has emerged as a new paradigm that improves the three key aspects of watermarking: payload (i.e., the number of bits embedded), robustness (i.e., the number of bits embedded), robustness (i.e., accuracy of the extracted message), and quality of embedded media. 

Previous research has focused on techniques such as lexical substitution with predefined rules and dictionaries or structural transformation. 

Recent work have either replaced the predefined set of rules with learning-based methodology, thereby removing heuristics or vastly improved the quality of lexical substitutions. 

A well-known proposition of a classical image watermarking work: That watermarks should *"be placed explicitly in the perceptually most significant components"*  of an image. If this is achieved, the adversary must corrupt the content's fundamental structure to destroy the watermark. This degrades the utility of the original content, rendering the purpose of pirating futile. 

Modification in individual pixels is much more imperceptible than on individual words. But to this, while we adhere to the gist of the proposition, we do not embed directly on the most significant component. Instead, we identify features that are semantically or syntactically fundamental components of the text and thus, invariant to minor modifications in texts. 




---


### CodeIPPrompt: Intellectual Property Infringement Assessment of Code Language Models 

* Authors : Zhiyuan Yu, Yuhao Wu, Ning Zhang, Chenguang Wang, Yevgeny Vorobeychik, Chaowei Xiao. 
* Published Date : 
* Record Date : 



---


### A Watermark for Large Language Models 

* Authors : John Kirchenbauer, Jonas Geiping, Yuxin Wen, Jonathan Katz, Ian Miers, Tom Goldstein 
* Published Date : 2023 ICML (Best Paper Award)
* Record Date : 2023.09.18

### Detecting the watermark

A third party with knowledge of the has function and random number generator can re-produce the red list for each token and count how many times the red list rule is violated. 

> $H_0$ : The text sequence is generated with no knowledge of the red list rule. 

<img src="https://drive.google.com/uc?export=view&id=1MA7qvM0p0L_lIWNUPqFxht7_iQ1r3yuw" style="width:100%">



<blockquote markdown="1">
### Spark Entropy 

the spike entropy of $p$ with modulus $z$ as 

$$
S(p,z) = \sum_k \frac{p_k}{1+z p_k}
$$

</blockquote>


---

<span class="spanbox"> Copyright </span>


### Evading Watermark based Detection of AI-Generated Content

* Authors : Zhengyuan Jiang, Jinghuai Zhang, Neil Zhenqiang Gong 
* Published Date : 2023.08.22  (Arvix) / ACM Conference on Computer and Communications Security (CCS), 2023 <d-cite key="jiang2023evading"/>
* Record Date : 2023.09.12 


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

---


> As threshold $\tau$ in a prediction increased, the false positive rate decreases as we have less number of positive predictions. 


#### Single-tail Detector 

An image $I$ is predicted as AI-generated if the bitwise accuracy of its decoded watermark is larger than a threshold $\tau$, i.e., $BA(D(I), w)> \tau$. 
Bitwise accuracy is computed by 

$$
BA(D(I_0), w) = \frac{m}{n}
$$

The service provider should pick the ground-truth watermark $w$ uniformly at random. 
Thus, the decoded watermark $D(I_0)$ is not related to the randomly picked $W$, and each bit of $D(I_0)$ is not related to the randomly picked $w$, and 
each bit of $D(I_0)$ matches with the corresponding bit of $w$ with probability 0.5. As a result, $m$ is a random variable and follows a binominal distribution $B(n, 0.5)$.  

Therefore, the FPR of the single-tail detector with threshold $\tau$ can be calculated as follows:

$$
\begin{aligned}
FPR (\tau) &= Pr(BA(D(I_0), w) > \tau) \\
          &= Pr(m > n\tau) = \sum_{k= \lceil n\tau \rceil}^n \begin{pmatrix} n \\ k \end{pmatrix} \frac{1}{2^k} \frac{1}{2^{n-k}}
\end{aligned}
$$

Thus, to make FPR rate less than $\eta$, we should find $\tau$ such that 

$$
\tau^{*} = \arg \min_{\tau}  \sum_{k= \lceil n\tau  \rceil}^n \begin{pmatrix} n \\ k \end{pmatrix} < \eta
$$
For instance, when $n=256$ and $\eta = 10^{-4}$, we have $\tau \ge \tau^* \approx 0.613$.

#### Double-tail Detector 

As adversarial attack can evade the watermark, the bitwise accuracy should remain at 0.5. 
The authors proposed a double-tail detector that detects an image $I$ asa  AI-generated if its decoded watermark ahs a bitwise accuracy 
larger than $\tau$ or smaller than $1-\tau$, i.e., $BA(D(I), w) > \tau$ or $BA(D(I), w) < 1-\tau$.
The FPR of the double-tail detector with threshold $\tau$ is:

$$
\begin{aligned}
FPR_{double}(\tau) &= Pr(BA(D(I_o), w)> \tau \operatorname{ or } BA(D(I_o), w) <  1-\tau) \\
                   &= Pr(m>n\tau \operatorname{ or } m < n -n\tau) = 2 \sum_{k=\lceil n\tau \rceil}^n \begin{pmatrix} n \\ k \end{pmatrix} \frac{1}{2^n}
\end{aligned}
$$





---

<span class="spanbox"> Copyright </span>

### Confidence-Building Measures for Artificial Intelligence: Workshop Proceedings 


* Authors : Sara Shoker, Andrew Reddie, et al., 
* Published Date : 2023.08.03 (Arxiv) <d-cite key="shoker2023confidencebuilding"/>
* Record Date : 2023.09.12 


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



---


<span class="spanbox"> Copyright </span>

### How to Protect Copyright Data in Optimization of Large Language Models? 

* Authors : Timothy Chu, Zhao Song, Chiwun Yang
* Published Date : 2023.08.23 (Arxiv) <d-cite key="chu2023protect"/>
* Record Date : 2023.09.11 



LLMs are built on the transformer neural network architecture, which in turn relies on a mathematical computation called Attention that uses the softmax function. \

To solve copyright regression for the softmax function, we show that the objective function of the softmax copyright regression is convex, and that its Hessian is bounded. 

* [Gil 19 ] investigates copyright infringement in AI-generated artwork and argues that using copyrighted works during the training phase of AI programs does not result in infringement liability. 

* [VKB23] proposes a frameowkr that provides stronger protection against sampling protected content, by defining near access-freeness (NAF)


<blockquote>
($\tau$-Copyright-Protected )
<br>

If there is a trained model $f_\theta$ with parameter $\theta$ that satisfies 

$$
\frac{L(f_\theta(A_1), b_1)}{n_1} \ge \tau + \frac{L(f_\theta(A_2)), b_2}{n_2}
$$

then we say this model $f_\theta$ is $\tau$-Copyright-Protected. 
</blockquote>


