---
---

<p align="center">
  <img src="https://user-images.githubusercontent.com/26765027/104627185-9cbe6400-568e-11eb-8d5f-3c0d7fab1dec.png" />
  <br>Welcome to a DevOps blog by Nick Otter.
  <br>Click the headers to view articles. Thanks for stopping by.
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

<div markdown="1">## DevOps

<div>
  <ul style="list-style-type:none">
    {% for post in site.categories.devops %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>
</div>

<div markdown="1">## Holistic

<div>
  <ul style="list-style-type:none">
    {% for post in site.categories.holistic %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>
</div>

<div markdown="1">## Being a Software Developer
   <ul style="list-style-type:none">
     {% for post in site.categories.software_developer %}
       {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
       {% endif %}
     {% endfor %}
    </ul>
 </div>

<div markdown="1">## System Design
   <ul style="list-style-type:none">
     {% for post in site.categories.sysdesign_handbook_contents %}
       {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
       {% endif %}
     {% endfor %}
    </ul>
 </div>

<div markdown="1">## Incident Management
   <ul style="list-style-type:none">
     {% for post in site.categories.incidents %}
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

<div markdown="1">## Docker

<div>
  <ul style="list-style-type:none">
    {% for post in site.categories.docker %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>
</div>

<div markdown="1">## Kubernetes
  <ul style="list-style-type:none">
    {% for post in site.categories.kubernetes %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>
</div>

<div markdown="1">## Linux
  <ul style="list-style-type:none">
    {% for post in site.categories.linux_handbook_contents %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
    {% endfor %}
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

<div markdown="1">## B-Sides

<div>
  <ul style="list-style-type:none">
    {% for post in site.categories.misc %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>
</div>
