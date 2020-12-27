---
layout: page
title: "Technical Content"
order: 2
permalink: technical
---

Here are my more technical past-projects or guides. If you find anything that's incorrect or have any improvements feel free to shoot me an email. 

{% assign pages_list = site.posts %}
{% for node in pages_list %}
{% if node.technical == true %}
  {% if node.layout == "post" %}
  	<a href="{{ node.url }}"> <b> {{ node.title }} </b> </a> <br>
  	<span> <i> {{ node.subtitle }} </i> </span> <br>
  	<span> {{ node.date | date: '%B %d, %Y'}} </span> <br>
  	{% if node.concepts %}
  	  	{% for concept in node.concepts %}
  	  	  	<span class="post-concept">{{ concept }}</span>
  	  	{% endfor %}
  	  	<br>
  	{% endif %}
  	<br>
  {% endif %}
{% endif %}
{% endfor %}