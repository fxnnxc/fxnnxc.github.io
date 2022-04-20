---
layout: page
permalink: /learning/
title: learning
description: Materials for courses you taught. Replace this text with your description.
nav: true
---

<!-- pages/learning.md -->
<div class="learning">
<h1>
Hello World
</h1>

  {%- assign sorted_projects = site.learning  -%}
  <!-- Generate cards for each project -->
  <div class="row">
      {% include learning.html %}
  </div>
