---
layout: default
title: Side Articles
description: Side Articles for several research topics
permalink: /side_articles/
---

<div class="post">
  <div class="header-bar">
    <h1>{{ page.title }}</h1>
    <h2>{{ page.description }}</h2>
  </div>
  <ul class="post-list">
    {%- assign sorted_pages = site.side_articles | sort: "date" %}
    {%- assign sorted_pages = sorted_pages | reverse -%} 

    {% for post in sorted_pages %}

    {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
    {% assign year = post.date | date: "%Y" %}
    {% assign tags = post.tags | join: "" %}
    {% assign categories = post.categories | join: "" %}

      <card class="column-first" style="border-bottom:#e4e4e4 solid; padding-left: 0px;" >
      <h2>
        {% if post.redirect == blank %}
          <a class="post-title" href="{{ post.url | prepend: site.baseurl }}" style="text-decoration:none; font-size: 20px; color:#362f5b ">{{ post.title }}</a>
        {% else %}
        <a class="post-title" href="{% if post.redirect contains '://' %}{{ post.redirect }}{% else %}{{ post.redirect | relative_url }}{% endif %}">{{ post.title }}</a>
        {% endif %}
      </h2>
      <p style="line-height: 140%;">{{ post.description }}</p>
      <p class="post-meta"> {{read_time}} min read &nbsp; &middot; &nbsp;
        {{ post.date | date: '%B %-d, %Y' }}
      
      <p class="post-tags">
        <a href="{{ year | prepend: '/blog/' | prepend: site.baseurl}}">
          <i class="fas fa-calendar fa-sm"></i> {{ year }} </a>

          {% if tags != "" %}
          &nbsp; &middot; &nbsp;
            {% for tag in post.tags %}
            <a href="{{ tag | prepend: '/blog/tag/' | prepend: site.baseurl}}">
              <i class="fas fa-hashtag fa-sm"></i> {{ tag }}</a> &nbsp;
              {% endfor %}
          {% endif %}

          {% if categories != "" %}
          &nbsp; &middot; &nbsp;
            {% for category in post.categories %}
            <a href="{{ category | prepend: '/blog/category/' | prepend: site.baseurl}}">
              <i class="fas fa-tag fa-sm"></i> {{ category }}</a> &nbsp;
              {% endfor %}
          {% endif %}
      </p>
    </p>
  </card>
      <card class="column-second">
      {% if post.img %}
          <img src="{{ post.img }}" width="145px" height="145px" style="margin-top:20px;margin-left:40px;border-radius: 20px;"> 
      {% endif %}
      </card>
    {% endfor %}
    
  </ul>
  {% include pagination.html %}

</div>