---
---

This is a DevOps blog by Nick Otter. Click each header to trigger a dropdown to view articles. Thanks for stopping by.
https://github.com/dear-github/dear-github/issues/166

<details><summary markdown='span'>Linux</summary><p>
</p></details>

<details open><summary>Microservices</summary><p>
## Kubernetes
    
   <ul style="list-style-type:none">
    {% for post in site.categories.kubernetes %}
      {% if post.url %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
    {% endfor %}
   </ul>

</p></details>

<details><summary markdown='span'>Misc</summary><p>
</p></details>
