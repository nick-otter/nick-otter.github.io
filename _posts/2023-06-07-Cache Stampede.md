---
layout: post
title:  "Cache Stampede"
categories: cache
---

# Cache Stampede
{: style="text-align: center"}

From [Alex Xu](https://www.linkedin.com/in/alexxubyte/).

# Introduction To Cache Stampede

Caching is awesome but it doesn’t come without a cost, just like many things in life.

One of the issues is *Cache Stampede*. Please correct me if this is not the right term. It refers to the scenario where data to fetch doesn't exist in the database and the data isn’t cached either. So every request hits the database eventually, defeating the purpose of using a cache. If a malicious user initiates lots of queries with such keys, the database can easily be overloaded.

The diagram below illustrates the process.

# Cache Stampede Diagram

![](/assets/cache_stampede.jpeg)

---