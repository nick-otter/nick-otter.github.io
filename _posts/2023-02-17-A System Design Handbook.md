---
layout: post
title:  "A System Design Handbook"
categories: sysdesign_handbook_contents
---

<div markdown="1"># A System Design Handbook

<div markdown="1"># General
  <div>
    <ul style="list-style-type:none">
      {% for post in site.categories.solutions %}
        {% if post.url %}
          <li><a href="{{ post.url }}">{{ post.title }}</a></li>
        {% endif %}
      {% endfor %}
    </ul>

<div markdown="1"># API
  <ul style="list-style-type:none">
    {% for post in site.categories.api %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1"># Latency
  <ul style="list-style-type:none">
    {% for post in site.categories.latency %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>
</div>
