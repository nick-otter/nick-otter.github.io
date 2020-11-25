# nick-otter.github.io

This is the repository for my DevOps blog which is currently hosted at `https://nick-otter.github.io/`.

It is written in `jekyll kramdown` and the current theme is no theme with some awkward HTML parsing. 

To run locally, use. 

```
$ jekyll serve
```

# Files

* `_posts` live posts with header format `YYYY-MM-DD-title`.
* `_config.yml` jekyll config file.
* `index.md` homepage.

# Jekyll front matter

Each post or homepage has a `yaml` `jekyll front matter` header for organisation.

E.g.

```yaml
---
layout: post
title:  "How to monitor Kubernetes with Prometheus"
categories: microservices kubernetes
---
```
# Categories

Categories are organised <primary> <secondary> and so on. 
  
So
```yaml
categories: microservices kubernetes
```
Primary is `microservices` and the secondary category is `kuberenetes`.

---

Thanks.
