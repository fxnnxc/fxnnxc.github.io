---
layout: default
description: Main Papers for Interpretability Research
title: Papers
permalink: /main_papers/
---
<div class="post">
  <div class="header-bar">
    <!-- <h2>{{ page.description }}</h2> -->
    <p align='left'>
    You can find the list of full papers in <strong> <a href="../share/full_paper_list/"> the full paper list </a>  </strong>
    </p>
  </div>
  <ul class="post-list">
    {%- assign sorted_pages = site.main_papers | sort: "date" %}
    {%- assign sorted_pages = sorted_pages | reverse -%} 

    {% for post in sorted_pages %}
    {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
    {% assign year = post.date | date: "%Y" %}
    {% assign tags = post.tags | join: "" %}
    {% assign categories = post.categories | join: "" %}
      <h2>
        {% if post.redirect == blank %}
          <a class="post-title" href="{{ post.url | prepend: site.baseurl }}" style="text-decoration:none; font-size: 20px; color:#362f5b ">{{ post.title }}</a>
        {% else %}
        <a class="post-title" href="{% if post.redirect contains '://' %}{{ post.redirect }}{% else %}{{ post.redirect | relative_url }}{% endif %}">{{ post.title }}</a>
        {% endif %}
      </h2>
      <div style="border-bottom:#e4e4e4 solid;display: grid;grid-template-columns: 3fr 3fr; " >
        <div>
      {% if post.img %}
          <img src="{{ post.img }}" width="100%" style="margin-top:10px;margin-bottom:10px;margin-left:0px;border-radius: 0px;padding-right:5px;"> 
      {% endif %}
      </div>
      <div>
<tag style="line-height: 120%;color:#00004B;font-family:Times New Roman; background-color:#F5F5FF;padding:5px; border-radius:15px;">{{ post.author_names }}</tag>
<p style="line-height: 140%;color:#00004B;font-family:Times New Roman;">{{ post.description }}</p>
      
      <p class="post-meta"> {{read_time}} min read &nbsp; &middot; &nbsp;
        {{ post.date | date: '%B %-d, %Y' }}
      </p>
    </div>

  </div>
    {% endfor %}
  </ul>
  {% include pagination.html %}

    {%- include footer.html %}
</div>