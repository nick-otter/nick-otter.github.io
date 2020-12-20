---
---

[![Foo](https://danashby04.files.wordpress.com/2016/10/model-1.jpg?w=820)](http://github.com/nick-otter)

This is a DevOps blog by Nick Otter. Click the arrow below to trigger a dropdown to view articles. Thanks for stopping by.


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

