---
layout: default
title: book
permalink: /book/
---


<div class="post">
  
  <ul class="post-list">
    {%- assign sorted_pages = site.book | sort: "date" %}
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
      <p style="line-height: 140%;">✍🏻 {{ post.description }}</p>
      <p class="post-meta"> &nbsp;
        {{ post.date | date: '%B %-d, %Y' }}
      
      <!-- <p class="post-tags">
        <a href="{{ year | prepend: '/blog/' | prepend: site.baseurl}}">
          <i class="fas fa-calendar fa-sm"></i> {{ year }} </a>

          {% if tags != "" %}
          &nbsp; &middot; &nbsp;
            {% for tag in post.tags %}
            
              <i class="fas fa-hashtag fa-sm"></i> {{ tag }} &nbsp;
              {% endfor %}
          {% endif %}

          {% if categories != "" %}
          &nbsp; &middot; &nbsp;
            {% for category in post.categories %}
            <a href="{{ category | prepend: '/blog/category/' | prepend: site.baseurl}}">
              <i class="fas fa-tag fa-sm"></i> {{ category }}</a> &nbsp;
              {% endfor %}
          {% endif %}
      </p> -->
    </p>
    <hr>

    {% endfor %}
    
  </ul>
  {% include pagination.html %}

</div>