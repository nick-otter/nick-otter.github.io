---
layout: post
title:  "How Kubernetes Manages StateLess & Stateful Applications"
categories: k8s_volume
---

# How Kubernetes Manages Stateless & Stateful Applications

From [Syed Nadeem](https://www.linkedin.com/posts/devops-with-syed_how-does-k8s-manage-stateless-stateful-activity-7099361836621266944-uq5V/?utm_source=share&utm_medium=member_android)


Based on whether the application requires persistent data and a stable identity for its components (usually pods), K8s has two main controllers that get used for application deployment & management.

📌 Deployment Controller - For StateLess Applications
Application that doesn't rely on maintaining local state or data across pod restarts. Each pod instance is interchangeable, and there's no requirement for preserving data between restarts.
They are extreamly easy to scale & manage. Pods created via this controllers come up significanlty faster then any other controllers.

📌 StatefulSet Controller - For StateFul Applications
Application that requires persistent storage and maintains some form of state or data that must be preserved across pod restarts or rescheduling. Perfect fix for pods that host DataBase components or key-value stores. Pods are created in a specific order and are given unique, stable network identities, even if they are rescheduled to different nodes or get restarted.

They are both crutial controllers for managing applications in K8s however you need to know which one to use where and how. Below picture will help you remember the key 
differences between the two controllers.

![](/assets/k8s_stateful.png)

---