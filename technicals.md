---
layout: page
title: "Technical Content"
order: 2
permalink: technical
---

Here are my more technical past-projects or guides. If you find anything that's incorrect or have any improvements feel free to shoot me an email. 

<ul class="list-of-posts">
{% for node in site.posts %}
{% if node.technical == true %}
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