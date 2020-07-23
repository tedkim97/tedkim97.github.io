---
layout: page
permalink: /
title: Home
order: 1
---

Welcome to my blog - here are some of my write-ups that I'm proud of! 

### My advice when reading:
Feel free to skip/skim parts that aren't interesting. I write these blog posts for completeness and detail to let people reproduce results. As a result, posts tend to contain a lot more detail (and are a lot longer) than necessary.

{% assign pages_list = site.posts %}
{% for node in pages_list %}
{% if node.frontpage == true %}
  {% if node.layout == "post" %}
  	<a href="{{ node.url }}"> <b> {{ node.title }} </b> </a> <br>
  	<span> <i> {{ node.subtitle }} </i> </span> <br>
  	<span> {{ node.date | date: '%B %d, %Y'}} </span>
  	<br> <br>
  {% endif %}
{% endif %}
{% endfor %}