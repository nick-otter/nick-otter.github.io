---
layout: post
title:  "Cheatsheet: Cache Types"
categories: cache
---

# Cheatsheet: Cache Types
{: style="text-align: center"}

Written by Nick Otter.

# Contents 

- [**Introduction**](#introduction)<br>
- [**Alex Xu**](#alex-xu)<br>

# Introduction

[This post](https://www.linkedin.com/posts/alexxubyte_systemdesign-coding-interviewtips-activity-7029127980660457472-ZX99/?utm_source=share&utm_medium=member_android) from [Alex Xu](https://www.linkedin.com/in/alexxubyte?miniProfileUrn=urn%3Ali%3Afs_miniProfile%3AACoAAAJcVUEBpKxeVUb94KnEePlKepfIXeP2RM0&lipi=urn%3Ali%3Apage%3Ad_flagship3_detail_base%3BEwCMDKCwQDuS7rGhgD1H%2FQ%3D%3D) is a great breakdown of Cache Scenarios, Techniques, Algorithms and Key Metrics.

# Alex Xu

Caching is one of the 𝐦𝐨𝐬𝐭 𝐜𝐨𝐦𝐦𝐨𝐧𝐥𝐲 used techniques when building fast online systems. When using a cache, here are the top 5 things to consider:

The first version of the cheatsheet was written by guest author Love Sharma.

𝐒𝐮𝐢𝐭𝐚𝐛𝐥𝐞 𝐒𝐜𝐞𝐧𝐚𝐫𝐢𝐨𝐬:
- In-memory solution
- Read heavy system
- Data is not frequently updated

𝐂𝐚𝐜𝐡𝐢𝐧𝐠 𝐓𝐞𝐜𝐡𝐧𝐢𝐪𝐮𝐞𝐬:
- Cache aside
- Write-through
- Read-through
- Write-around
- Write-back

𝐂𝐚𝐜𝐡𝐞 𝐄𝐯𝐢𝐜𝐭𝐢𝐨𝐧 𝐀𝐥𝐠𝐨𝐫𝐢𝐭𝐡𝐦𝐬:
- Least Recently Used (LRU)
- Least Frequently Used (LFU)
- First-in First-out (FIFO)
- Random Replacement (RR)

𝐊𝐞𝐲 𝐌𝐞𝐭𝐫𝐢𝐜𝐬:
- Cache Hit Ratio
- Latency
- Throughput
- Invalidation Rate
- Memory Usage
- CPU usage
- Network usage

𝐎𝐭𝐡𝐞𝐫 𝐢𝐬𝐬𝐮𝐞𝐬:
- Thunder herd on cold start
- Time-to-live (TTL)

![](/assets/cache.jpg)

---

Thanks. This was written by Nick Otter.
