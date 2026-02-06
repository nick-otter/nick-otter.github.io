---
layout: post
title:  "Kubernetes: Architecture"
tags: kubernetes
---

| Alex Xu | [Alex Xu](https://www.linkedin.com/posts/alexxubyte_systemdesign-coding-interviewtips-activity-7078759302592634880-Zfb-/?utm_source=share&utm_medium=member_android)|

k8s is a container orchestration system. It is used for container deployment and management. Its design is greatly impacted by Google’s internal system Borg.

A k8s cluster consists of a set of worker machines, called nodes, that run containerized applications. Every cluster has at least one worker node.

The worker node(s) host the Pods that are the components of the application workload. The control plane manages the worker nodes and the Pods in the cluster. In production environments, the control plane usually runs across multiple computers and a cluster usually runs multiple nodes, providing fault-tolerance and high availability.

# Control Plane Components

1. **API Server**
The API server talks to all the components in the k8s cluster. All the operations on pods are executed by talking to the API server.

1. **Scheduler**
The scheduler watches the workloads on pods and assigns loads on newly created pods.

1. **Controller Manager**
The controller manager runs the controllers, including Node Controller, Job Controller, EndpointSlice Controller, and ServiceAccount Controller.

1. **etcd**
etcd is a key-value store used as Kubernetes' backing store for all cluster data.

# Nodes
1. **Pods**
A pod is a group of containers and is the smallest unit that k8s administers. Pods have a single IP address applied to every container within the pod.

2. **Kubelet**
An agent that runs on each node in the cluster. It ensures containers are running in a Pod.

3. **Kube Proxy**
kube-proxy is a network proxy that runs on each node in your cluster. It routes traffic coming into a node from the service. It forwards requests for work to the correct containers.

![](/assets/k8_overview.jpg)
