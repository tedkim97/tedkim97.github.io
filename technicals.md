---
layout: page
title: "Serious Content"
order: 2
permalink: technical
---

Here are my more technical past-projects/mini projects. If you find anything that's incorrect or have any improvements feel free to shoot me an email. 

{% assign pages_list = site.posts %}
{% for node in pages_list %}
{% if node.technical == true %}
  {% if node.layout == "post" %}
  	<a href="{{ node.url }}">{{ node.title }}</a><br>
  {% endif %}
{% endif %}
{% endfor %}