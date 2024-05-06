---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
bibliography: all.bib
disqus_comments: false
giscus_comments: false
date: 2024-05-05
featured: true
title: 'Towards Selecting Document Memories from Token Representations'
description: '문서에 대한 엔트리를 만드는 것과 실제 토큰이 엔트리를 접근하고 사용하는지 여부는 불확실하다. 이 실험에서는 토큰으로부터 문서 메모리 선택의 방식을 관찰하고 토큰으로부터 문서 메모리를 end-to-end로 선택하는 방식을 연구한다. '
---


토큰은 메모리에 대한 선택을 책임진다. 

1. LLM이 제공하는 토큰의 표현을 변형해야 한다. 