---
layout: page
title: Hobbies & Fun Stuff
order: 3
permalink: goofing_around
---

Here are my past projects relating to my hobbies, off-shoot projects, or musings. The content on this page is pretty diverse, so not everything in this page will appeal to you.

<ul class="list-of-posts">
{% for node in site.posts %}
{% if node.funstuff == true %}
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