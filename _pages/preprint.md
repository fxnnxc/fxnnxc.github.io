---
layout: page
permalink: /preprint/
title: Preprint
description: Materials for courses you taught. Replace this text with your description.
nav: false
---

<!-- pages/preprint.md -->
<div class="preprint">
<h1>
Hello World
</h1>
  {%- assign sorted_projects = site.preprint  -%}
  <!-- Generate cards for each project -->
  <div class="container">
    <div class="row row-cols-2">
    {%- for project in sorted_projects -%}
      {% include preprint.html %}
    {%- endfor %}
    </div>
  </div>
