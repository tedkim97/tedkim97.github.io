---
layout: page
permalink: /
title: Home
order: 1
---

Welcome to my blog - here are some of my write-ups that I'm proud of!

{% assign pages_list = site.posts %}
{% for node in pages_list %}
{% if node.frontpage == true %}
  {% if node.layout == "post" %}
  	<a href="{{ node.url }}">{{ node.title }}</a><br>
  {% endif %}
{% endif %}
{% endfor %}