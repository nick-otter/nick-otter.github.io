---
---

<p align="center">
  <img src="https://user-images.githubusercontent.com/26765027/104627185-9cbe6400-568e-11eb-8d5f-3c0d7fab1dec.png" />
  <br>Welcome to the minimal DevOps blog by Nick Otter. Click the headers to view articles. Thanks for stopping by.
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
    <li><a href="https://www.linkedin.com/in/nick-otter/">LinkedIn</a></li>
  </ul>
</div>

<div markdown="1">## Personal

<div>
  <ul style="list-style-type:none">
    {% for post in site.categories.personal %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>
</div>

<div markdown="1">## System Design

<div markdown="1">### General
  <div>
    <ul style="list-style-type:none">
      {% for post in site.categories.solutions %}
        {% if post.url %}
          <li><a href="{{ post.url }}">{{ post.title }}</a></li>
        {% endif %}
      {% endfor %}
    </ul>

<div markdown="1">### API
  <ul style="list-style-type:none">
    {% for post in site.categories.api %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1">### Databases
  <ul style="list-style-type:none">
    {% for post in site.categories.databases %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1">### Latency
  <ul style="list-style-type:none">
    {% for post in site.categories.latency %}
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

<div markdown="1">### Cheatsheets
  <ul style="list-style-type:none">
    {% for post in site.categories.kubernetes_cheatsheets %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1">### Prometheus
  <ul style="list-style-type:none">
    {% for post in site.categories.prometheus %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>
</div>

<div markdown="1">## Linux

<div markdown="1">### General
<div>
  <ul style="list-style-type:none">
    {% for post in site.categories.linux %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1">### Boot
  <ul style="list-style-type:none">
    {% for post in site.categories.boot %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1">### Network
  <ul style="list-style-type:none">
    {% for post in site.categories.network %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1">### Kernel
  <ul style="list-style-type:none">
    {% for post in site.categories.kernel %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1">### Disk
  <ul style="list-style-type:none">
    {% for post in site.categories.disk %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1">### Memory
  <ul style="list-style-type:none">
    {% for post in site.categories.memory %}
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

