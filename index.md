---
layout: page
permalink: /
title: Home
order: 1
---

Welcome to my blog - here are some of my write-ups that I'm proud of! Feel free to skip/skim parts that aren't interesting to you. I try to write these blog posts for reproducibility (i.e a lot of detail and code snippets), so posts contain more detail than necessary.

<ul class="list-of-posts">
{% for node in site.posts %}
{% if node.frontpage == true %}
  {% if node.layout == "post" %}
  <li class="sans-marker">
  	<a href="{{ node.url }}"> <b> {{ node.title }} </b> </a> <br>
  	<span> <i> {{ node.subtitle }} </i> </span> <br>
  	{% comment %}
  	<span> {{ node.date | date: '%B %d, %Y'}} </span> <br>
  	{% endcomment %}
  	{% if node.concepts %}
  	  {% for concept in node.concepts %}
  	  	<span class="post-concept">{{ concept }}</span>
  	  {% endfor %}
  	  <br>
  	{% endif %}
  </li>
  {% endif %}

{% endif %}
{% endfor %}
</ul>