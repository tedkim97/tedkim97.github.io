---
layout: page
title: Hobbies & Fun Stuff
order: 3
permalink: goofing_around
---

Here are my past projects relating to my hobbies, off-shoot projects, or musings. The content on this page is pretty diverse, so not everything in this page will appeal to you.

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