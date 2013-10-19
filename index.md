---
layout: page
title: Wade Wegner's Blog
tagline: 
---
{% include JB/setup %}

<h3>{{ site.tagline }}</h3>

Recent posts:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

