---
layout: post
title:  "A Kubernetes Handbook"
categories: kubernetes
---

# A Kubernetes Handbook
{: style="text-align: center"}

<div markdown="1"># Overviews
<div>
  <ul style="list-style-type:none">
    {% for post in site.categories.k8s_overviews %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1"># Volume
<div>
  <ul style="list-style-type:none">
    {% for post in site.categories.k8s_volume %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>   