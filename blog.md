---
layout: default
title: Theo Olausson
subtitle: Blog
---
{% for post in site.posts %}
<h3 style="padding-bottom: 0px; margin-bottom: 0px;"><a href="{{ post.url }}">{{ post.title }}</a></h3>
<small style="padding-top: 0px; margin-top: 0px;"> {{ post.date | date: "%A, %B %d, %Y" }} </small>
<p> {{ post.excerpt | truncate: 300 }} </p>
{% endfor %}
