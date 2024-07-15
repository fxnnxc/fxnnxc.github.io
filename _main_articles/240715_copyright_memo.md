---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
giscus_comments: true
disqus_comments: false
date: 2024-07-15
featured: true
title: 'Review on "The Files are in the Computer: Copyright, Memorization, and Generative-AI Systems"'
description: ''
_styles: >
    .table {
        padding-top:200px;
        margin-bottom:  2.5rem;
        border-bottom: 2px;
    }
    .p {
        font-size:20px;
    }
    .center{
        display: block;
        margin-left: auto;
        margin-right: auto;
        width: 50%;
    }
---

The Files are in the Computer: Copyright, Memorization, and Generative-AI Systems


All generative-AI models memorize some portion of their training data.


This work use the term, regurgitation to describe the behavior of LLMs which just swallow information and bring the information back up to the end user without vomiting.  


What a model memorizes: 
1. it could memorize complete training examples,
2. it could memorize isolated facts (such as social security numbers), 
3. common information present in many examples (such as the features of a celebrity’s face)


### Definitions 
1. Extraction: when a user intentionally and successfully prompts a model to generate an exact or near-exact copy of a piece of training data. (extraction is regurgitation plus intent.)
2. Regurgitation: a model generates an output that is an exact or near-exact copy of piece of training data
3. Memorization: When an exact or near-exact copy of **a piece of training data can be reconstructed by examining the model through any means**
4. Reconstruction: The processes, which can include but are not limited to prompting.


LLMs can be prompted to output near-exact copies of training data.


* Memorization is a front-end phenomenon; it describes characteristics and capabilities of the model itself that directly result from its training.
* Regurgitation and extraction are back-end-phenomena; they describe how the model behaves in generating outputs in response to a specific prompt

Not all memorization is regurgitation or extraction; material can
be present in a model but inaccessible through prompting with a particular
strategy.

For an (imperfect) analogy, consider the Google Books database. Google’s corpus of scanned books includes complete images of every page from the books it has scanned; treating the corpus like a model, it would be a straightforward example of memorization. Google allows searchers on Google Books to view “snippets” of an eighth of a page containing their search terms, which would be straightforward regurgitation and extraction

greater ability to restrict its outputs to prevent how much copied content is surfaced to the user.


### The Copyright Act

“copies” of a copyrightable work are “objects . . . from which the work can be perceived,
reproduced, or otherwise communicated.”

### Reguritation is Copying
if I have Blu-Ray disc of Barbie (2023), it is a “copy” of the audiovisual work Barbie, because it can be “perceived” by playing it in a Blu-Ray player. If I rip the disc to an SSD storage device, the SSD also becomes a “copy” of Barbie; it can be “perceived” by playing it with software like QuickTime Player. It is still a “copy” even if I downscale it to a lower resolution and change the file format; copies do not have to have exactly the same information or the same encoding.

The legal definition of “copy” is functional: a “copy” of a work is defined by the fact that one can reconstruct enough of the work from it.

To say that regurgitation is copying does not necessarily mean that it is copyright infringement. A model might regurgitate unembellished, uncopyrightable material, like the factual alphabetized list of the fifty U.S. states


### Regurgitation Implies Memorization

Means that when regurgitation happens, the model memorized the content. The model stored information in any means 

memorization takes place when a piece of training data can be emitted from a model by any means, and prompting is one such means. But there is a deeper point here. The definitions of extraction and regurgitation focus attention on the generation of outputs.



“repeat after me” is not useful evidence of memorization. Here, the model repeats the text from the monologue in the opening credits of Star Trek: The Next Generation. Interaction produced by the authors.


For example, OpenAI has claimed
that its alignment techniques successfully trained its ChatGPT models to
avoid memorization.92 But in late 2023, a team led by Google DeepMind researchers
developed a new technique and conducted a large-scale measurement
study that showed the ChatGPT 3.5 (turbo endpoint) model memorized
significantly more training data than any other model they tested.


The right way to describe this situation is that this work showed that ChatGPT
3.5 had memorized training data all along—and not that ChatGPT suddenly
went from not memorizing training data to memorizing it just because this
research team devised a way to get it out of the system

Similarly, the Copyright Act uses the phrase “now known or later developed”
to describe the reconstruction of a work from a copy. It is possible
that a model is currently a copy of some of its training data, even though
the techniques for extracting it will only be developed in the future.

For another example (also involving advances in machine learning), ancient papyrus scrolls buried in the eruption of Mt. Vesuvius in 79 C.E. were discovered 275 years ago but were too fragile to physically unroll without collapsing into a pile of ash.96 Scholars have recently successfully used computer tomography to generate images of the interior of the rolled-up scrolls and machine learning to identify the letters on them. The scrolls were fixed copies all this time, but we did not have definitive proof until this year.

Thus, a little unfortunately, it is often the case that generative-AI developers, users, and commentators must live in a state of ignorance about whether a model has memorized its training data. A successful extraction or an instance of regurgitation can provide positive proof that it has memorized. In some cases, it may be possible to show that a model has not memorized certain data because these data were definitely not present in the training dataset.97 But in between there is a middle ground of greater and lesser ignorance: there is a fact of the matter, and we can have good reasons for thinking that a model has or has not memorized with a given degree of confidence, but certainty is not to be had.

But that is the wrong test, because the mere fact that a model is encoded
in a way that is not directly intelligible to the human senses is irrelevant.
All digital media are encoded in ways that are not directly intelligible,
twice over. Consider the png image file of the McDuck panel that we include
in this essay, or the PDF version of our essay that you are currently
reading.

It is only the relative novelty of generative-AI models

Similarly, Anthropic argues that Claude’s model parameters contain “statistical
correlations” that “ . . . effectively yield ‘insights about patterns of connections
among concepts or how works of [a particular] kind are constructed.”’

Instead of memorizing each individual training example,
the model learns only the abstracted pattern—a more succinct description
of some way in which multiple training examples resemble each other, or
parts of an example are repeated.

But this contrast can be misleading when used this way. The problem
is that the “patterns” learned by a model can be highly abstract, highly specific,
or anywhere in between. It is a “pattern” that every frame of 4K UHD
video is 3840 pixels wide; a multi-modal model that learns to generate images
of the correct resolution when the prompt contains “4k uhd” has not memorized
any protectable expression. But it is also a “pattern” that in the first
sentence of The Restaurant at the End of the Universe the word “widely” is followed
by the word “regarded,” and a model that learns enough such patterns
can memorize all of Douglas Adams’s oeuvre. So even to the extent that the
results of model training are made up of “patterns,” so is all memorization,
because memorization is part of what happens during training.

A more accurate distinction would be that some learned patterns contain
higher-order information that is not present in any single training example.
Thus, for example, somewhere in a trained image-generation model’s parameters,
there may be information about the length of cats’ whiskers gleaned
from numerous pictures of cats. In contrast, for memorization, somewhere distributed among the parameters, there is a literal copy of a specific piece of training data—a specific picture of a cat.

**Then, in-context learning may be a good approach for the copyright issues because the model parameter does not memorize the content, but the generation procedure done by a user reproduce the copyrighted content.**

“The models we have for understanding the entire arena of the patentability of algorithms are
inadequate—not just somewhat inadequate, but fundamentally so. They are broken.”


Allen Newell, Response: The Models Are Broken; The Models Are Broken, 47 U. Pitt. L.
Rev. 1023, 1023 (1986).

Newell’s warning has renewed force today. Courts, regulators, and
scholars who are grappling with how to apply existing laws to Generative
AI—or formulate new ones—must build their theories atop a foundation of
conceptual models of how generative-AI systems work, with respect to memorization
and much else. If they do not, faulty technical assumptions will lead
to ungrounded legal claims—not necessarily wrong, but with no reliable connection
to the underlying systems they purport to describe. They need, in
short, a good model of models.


### 알고리즘의 특허 가능성을 이해하고 평가하는 데 사용되는 기본 개념적 틀이나 사고 방식을 나타냅니다.

Allen Newell의 "Response: The Models Are Broken; The Models Are Broken"이라는 글은 알고리즘의 특허 가능성에 대한 논의와 관련된 기본 개념적 모델들이 근본적으로 잘못되었다는 주장을 담고 있습니다. 주요 내용은 다음과 같습니다:

알고리즘과 특허 가능성에 대한 혼란:

Newell은 알고리즘과 그 특허 가능성에 관한 혼란이 본질적으로 우리가 사용하는 개념적 모델에서 비롯된다고 주장합니다.
이러한 모델들이 알고리즘의 특성이나 그 사용에 대해 적절하게 설명하지 못하고 있다는 점을 강조합니다.
Benson 사건의 예:

Benson 사건이 어떤 결론을 내렸든 간에, 즉 특정 알고리즘이 특허 가능하다고 판정되든 특허 불가능하다고 판정되든, 혼란이 발생했을 것이라고 언급합니다.
이는 알고리즘과 특허 가능성을 논의할 때 사용되는 모델 자체가 불충분하다는 것을 보여줍니다.
기본 모델의 부적절성:

Newell은 우리가 알고리즘의 특허 가능성을 이해하기 위해 사용하는 모델이 단지 약간 부적절한 것이 아니라 근본적으로 부적절하다고 주장합니다.
이러한 모델들의 문제로 인해 알고리즘과 특허 가능성에 대한 명확한 이해와 판단이 어려워진다고 설명합니다.
이 글의 주요 내용은 알고리즘의 특허 가능성을 논의할 때 사용되는 현재의 개념적 모델들이 근본적으로 잘못되었으며, 이러한 모델들의 한계를 인식하고 개선해야 한다는 것입니다.


### Gottschalk v. Benson

Benson 사건은 미국 대법원이 다룬 Gottschalk v. Benson 사건(1972)을 말합니다. 이 사건은 컴퓨터 소프트웨어와 관련된 특허법 해석에 중요한 영향을 미쳤습니다. 다음은 Benson 사건에 대한 주요 내용입니다:

사건 배경:

발명: Walter Benson과 그의 동료들은 이진 소수를 이진 코드로 변환하는 방법을 발명했습니다. 이 변환 알고리즘은 컴퓨터에서 실행될 수 있는 수학적 알고리즘이었습니다.
특허 신청: 이 발명을 특허로 보호받기 위해 특허 신청을 했습니다.
법적 쟁점:

특허 가능성: 사건의 핵심은 이러한 수학적 알고리즘이 특허법 상 특허로 보호받을 수 있는지 여부였습니다.
미국 특허법: 미국 특허법은 자연법칙, 물리적 현상, 추상적 아이디어는 특허로 보호받을 수 없다고 명시하고 있습니다.
대법원 판결:

결정: 미국 대법원은 Benson 사건에서 이진 소수를 이진 코드로 변환하는 알고리즘은 특허로 보호받을 수 없다고 판결했습니다.
이유: 대법원은 이 알고리즘이 단순히 수학적 공식을 컴퓨터에서 실행하는 것이며, 이는 추상적 아이디어에 해당한다고 판단했습니다. 따라서, 특허법으로 보호받을 수 없다고 결정했습니다.
영향:

소프트웨어 특허: Benson 사건은 이후 컴퓨터 소프트웨어 및 알고리즘 특허와 관련된 판례와 법적 기준에 큰 영향을 미쳤습니다.
추상적 아이디어: 이 사건은 추상적 아이디어가 특허로 보호받을 수 없다는 원칙을 강화했으며, 이후의 소프트웨어 특허와 관련된 법적 논의에 중요한 참조점이 되었습니다.
Benson 사건은 알고리즘과 소프트웨어의 특허 가능성에 대한 법적 기준을 설정하는 데 중요한 역할을 했으며, 이는 컴퓨터 과학 및 법률 분야에서 여전히 중요한 판례로 남아 있습니다.

소수 부분에 2를 곱해가면서 나타나는 정수 0 또는 1을 기록하여 소수를 기록하는 알고리즘 

Allen Newell, Response: The Models Are Broken; The Models Are Broken, 47 U. Pitt. L.
Rev. 1023, 1023 (1986).