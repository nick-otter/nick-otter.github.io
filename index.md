---
---

![Image thanks to Dan Ashby athttps://danashby.co.uk/2016/10/19/continuous-testing-in-devops/](https://user-images.githubusercontent.com/26765027/102725400-5df6f180-430e-11eb-89b0-5e967c656317.png)

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

