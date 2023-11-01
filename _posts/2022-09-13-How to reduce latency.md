---
layout: post
title:  "How To Reduce Latency"
categories: latency
---

# How To Reduce Latency 
{: style="text-align: center"}

From [John Crickett](https://www.linkedin.com/posts/johncrickett_what-to-know-the-secret-to-reducing-latency-activity-7120310703164399616-qdFL/?utm_source=share&utm_medium=member_android).

What to know the secret to reducing latency in any software system?

That works at every level.

It’s simple: move things close to each other.

Latency too high when sending data from a server in New York to a server in San Francisco - move the data to a server in San Francisco.

Latency too high when doing high frequency trading in City of London 1km from the stock exchange - move the trading engine into the stock exchange’s own data centre, just metres from the exchange’s own servers.

Latency too high reading data from a hard drive - move the data closer to the CPU, by read-ahead buffering (aka caching).

Latency too high when reading from main memory for a high performance compute engine - move the data closer to the CPU by caching it in the L1 and L2 cache.

Latency too high when reading cache, move the data closer still - keep it in a register.

In every case, we reduce latency by moving the data nearer to the processing. Sometimes we permanently move it, other times we temporarily move it using caches.

---