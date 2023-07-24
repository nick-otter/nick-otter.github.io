---
layout: post
title:  "An AWS Handbook"
categories: aws
---

<div markdown="1"># An AWS Handbook

<div markdown="1"># Introduction
  <div>
    <ul style="list-style-type:none">
      {% for post in site.categories.aws_links %}
        {% if post.url %}
          <li><a href="{{ post.url }}">{{ post.title }}</a></li>
        {% endif %}
      {% endfor %}
      {% for post in site.categories.aws_overview %}
        {% if post.url %}
          <li><a href="{{ post.url }}">{{ post.title }}</a></li>
        {% endif %}
      {% endfor %}
    </ul>

<div markdown="1"># Storage
  <ul style="list-style-type:none">
    {% for post in site.categories.aws_storage %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1"># Databases
  <ul style="list-style-type:none">
    {% for post in site.categories.aws_databases %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1"># Data Pipelines
  <ul style="list-style-type:none">
    {% for post in site.categories.aws_datapipelines %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1"># Operations
  <ul style="list-style-type:none">
    {% for post in site.categories.aws_operations %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1"># Network
  <ul style="list-style-type:none">
    {% for post in site.categories.aws_network %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

<div markdown="1"># Compute
  <ul style="list-style-type:none">
    {% for post in site.categories.aws_compute %}
      {% if post.url %}
         <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endif %}
     {% endfor %}
   </ul>

---