---
layout: page
title: Lewis Chung
tagline: Supporting tagline
---
{% include JB/setup %}
{% include nav %}

<h1 style="font-size: 1.5em; font-weight: 300; margin-bottom: 1.2em">Blog Posts</h1>
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo;&nbsp;&nbsp;&nbsp; <a href="{{ BASE_PATH }}{{ post.url }}" class="tk-museo-sans">{{ post.title }}</a></li>
  {% endfor %}
</ul>



