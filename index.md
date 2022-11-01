---
---

<p align="center">
  <img src="https://user-images.githubusercontent.com/26765027/104627185-9cbe6400-568e-11eb-8d5f-3c0d7fab1dec.png" />
  <br>Welcome to a DevOps blog by Nick Otter. Click the headers to view articles. Thanks for stopping by.
</p>

  
<div markdown="1">## Info

<div>
  <ul style="list-style-type:none">
          {% post_url 2020-02-18-about-me %} 
         <li><a href="https://github.com/nick-otter/">Github</a></li>
         <li><a href="https://www.linkedin.com/in/nick-otter/">LinkedIn</a></li>
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

<div markdown="1">## Amazon Web Services

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

