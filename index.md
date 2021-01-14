---
---

<p align="left">
  <img src="https://user-images.githubusercontent.com/26765027/104597240-fc0b7c80-566c-11eb-81b1-7b570b349292.png" />
</p>


Welcome to a DevOps blog by Nick Otter. Click the arrow below to trigger a dropdown to view articles. Thanks for stopping by.


<details><summary></summary>
  
<div markdown="1">## Links

<div>
  <ul style="list-style-type:none">
         <li><a href="https://github.com/nick-otter/">Github</a></li>
  </ul>
</div>

<div markdown="1">## Kubernetes

<div>
  <ul style="list-style-type:none">
    {% for post in site.categories.kubernetes %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>
</div>

<div markdown="1">## Linux

<div>
  <ul style="list-style-type:none">
    {% for post in site.categories.linux %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>
</div>

<div markdown="1">## Misc

<div>
  <ul style="list-style-type:none">
    {% for post in site.categories.misc %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>
</div>

