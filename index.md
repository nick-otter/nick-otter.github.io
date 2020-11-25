---
---

This is a DevOps blog by Nick Otter. Click the icon to trigger a dropdown to view articles. Thanks for stopping by.


<details><summary></summary>

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


