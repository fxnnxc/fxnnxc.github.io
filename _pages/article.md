---
layout: page
permalink: /article/
title: ✍🏻 article
description: Personal posts
nav: true
order : 7
---


🍀 You can see literature reviews for specific research topics in [Literature Reviews](/reviews/) 


----

🍀 Here are topic specific contents


<!-- pages/article.md -->
<div class="article">
  {%- assign sorted_projects = site.article  -%}
  <!-- Generate cards for each project -->
  <div class="row">
      {% include article.html %}
  </div>

</div>