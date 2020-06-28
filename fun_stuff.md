---
layout: page
title: Fun Stuff & Hobbies
order: 3
permalink: tomfoolery
---

Here are my past projects relating to my hobbies or off-shoot projects. Not everything in this page will appeal to you, and that's fine!

{% assign pages_list = site.posts %}
{% for node in pages_list %}
{% if node.funstuff == true %}
  {% if node.layout == "post" %}
  	<a href="{{ node.url }}">{{ node.title }}</a><br>
  {% endif %}
{% endif %}
{% endfor %}