---
---

<p align="center">
  <img src="https://user-images.githubusercontent.com/26765027/104627185-9cbe6400-568e-11eb-8d5f-3c0d7fab1dec.png" />
  <br>Welcome to a DevOps blog by Nick Otter. Click the headers to view articles. Thanks for stopping by.
</p>

  
<div markdown="1">## Info

<div>
  <ul style="list-style-type:none">
         {% for post in site.categories.info %}
           {% if post.url %}
              <li><a href="{{ post.url }}">{{ post.title }}</a></li>
           {% endif %}
         {% endfor %}
         <li><a href="https://github.com/nick-otter/">Github</a></li>
  </ul>
</div>

<div markdown="1">## Solutions Architecture

<div>
  <ul style="list-style-type:none">
    {% for post in site.categories.solutions %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>
</div>

<div markdown="1">## AWS

<div>
  <ul style="list-style-type:none">
    {% for post in site.categories.aws %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
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

