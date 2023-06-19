---
layout: post
title:  "Common Load Balancing Strategies"
categories: loadbalancing
---

# Common Load Balancing Strategies
{: style="text-align: center"}

From [Leo Coelho](https://www.linkedin.com/in/leo-coelho/).

# Introduction

The primary responsibility of a Load Balancer is to evenly distribute incoming network traffic across multiple servers or containers.

👉 When an incoming request (external traffic) reaches the #apigateway, it forwards the request to the #loadbalancer. Then:

1️⃣ The Load Balancer applies its load balancing strategy to select an available container to handle the request.

2️⃣ The Load Balancer routes the request to the chosen container running the application.

3️⃣ The container processes the request, generates a response, and sends it back to the client (via the Load Balancer and API Gateway).

Here are some common load-balancing strategies.

# Round Robin

1️⃣ Round Robin: distributes sequentially across the available containers.

# Least Connection 

2️⃣ Least Connection: forwards to the container with the fewest active connections.

# IP Hash

3️⃣ IP Hash: use the client's IP address to determine the container to handle the request. It ensures that a given client always reaches the same container.

# Weighted Round Robin

4️⃣ Weighted Round Robin: Each container is assigned a weight, and requests are distributed based on the assigned weights. Containers with higher weights receive a proportionally larger share of requests.

---