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
description: 'Copyright is one of the most important issues in the realm of generative AI. '
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

This post is a personal review on the paper, 
"The Files are in the Computer: Copyright, Memorization, and Generative-AI Systems". 

One of the thrilling sentence was 

> All generative-AI models memorize some portion of their training data.

This work provides clear definition on the memorization, regurgitation, and extraction and provides insights how to view the memorized copyright contents. However, I think some possible solutions of copyright issues based this essay are nearly none. As we observe the hardness of the problem, I feel like the answers of the problems don't exist. One reason is that the targets, which are a form of knowledge, to be protected are vague and they lost there concrete discrete form which exists before generative AIs play the social game. 


This work use the term, regurgitation to describe the behavior of LLMs which just swallow information and bring the information back up to the end user without vomiting.  


What a model memorizes: 
1. it could memorize complete training examples,
2. it could memorize isolated facts (such as social security numbers), 
3. common information present in many examples (such as the features of a celebrity’s face)


### Definitions 
1. **Extraction**: when a user intentionally and successfully prompts a model to generate an exact or near-exact copy of a piece of training data. (extraction is regurgitation plus intent.)
2. **Regurgitation**: a model generates an output that is an exact or near-exact copy of piece of training data
3. **Memorization**: When an exact or near-exact copy of **a piece of training data can be reconstructed by examining the model through any means**
4. **Reconstruction**: The processes, which can include but are not limited to prompting.

> LLMs can be prompted to output near-exact copies of training data.


* Memorization is a front-end phenomenon; it describes characteristics and capabilities of the model itself that directly result from its training.
* Regurgitation and extraction are back-end-phenomena; they describe how the model behaves in generating outputs in response to a specific prompt

Not all memorization is regurgitation or extraction; material can be present in a model but inaccessible through prompting with a particular strategy.

For an (imperfect) analogy, consider the Google Books database. Google’s corpus of scanned books includes complete images of every page from the books it has scanned; treating the corpus like a model, it would be a straightforward example of memorization. Google allows searchers on Google Books to view “snippets” of an eighth of a page containing their search terms, which would be straightforward regurgitation and extraction


### The Copyright Act

> “copies” of a copyrightable work are “objects . . . from which the work can be perceived, reproduced, or otherwise communicated.”

### Regurgitation is Copying

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

