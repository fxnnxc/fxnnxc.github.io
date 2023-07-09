---
layout: page
permalink: /publications/
title: 📚 Paper
description: these are published papers
years: [2022, 2021]
nav: true
order : 6
---

<a href="https://scholar.google.co.kr/citations?user=XzIXaxoAAAAJ&hl=ko"> Bumjin's Google Scholar </a>

<!-- _pages/publications.md -->
<div class="publications">

{%- for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>
