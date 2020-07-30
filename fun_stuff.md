---
layout: page
title: Fun Stuff & Hobbies
order: 3
permalink: tomfoolery
---

Here are my past projects relating to my hobbies, off-shoot projects, or musings. Not everything in this page will appeal to you, and that's fine!

{% assign pages_list = site.posts %}
{% for node in pages_list %}
{% if node.funstuff == true %}
  {% if node.layout == "post" %}
  	<a href="{{ node.url }}"> <b> {{ node.title }} </b> </a> <br>
  	<span> <i> {{ node.subtitle }} </i> </span> <br>
  	<span> {{ node.date | date: '%B %d, %Y'}} </span>
  	<br> <br>
  {% endif %}
{% endif %}
{% endfor %}