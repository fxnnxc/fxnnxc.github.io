---
layout: page
permalink: /knowledge/
title: 🌀 knowledge
description: Materials for courses you taught. Replace this text with your description.
nav: true
---

<!-- pages/knowledge.md -->
<div class="knowledge">
  {%- assign sorted_projects = site.knowledge  -%}
  <!-- Generate cards for each project -->
  <div class="row">
      {% include knowledge.html %}
  </div>
