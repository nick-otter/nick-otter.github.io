---
layout: post
title:  "CAP theorem for your Solutions Architecture"
categories: solutions
---

# CAP theorem for your Solutions Architecture
{: style="text-align: center"}

From Alex Xu at ByteByteGo.

[Alex Xu](https://www.linkedin.com/in/alexxubyte/) at [ByteByteGo](https://bytebytego.com/) recently published ["CAP theorem: one of the most misunderstood terms"](https://www.linkedin.com/feed/update/urn:li:activity:6980914617103360000/).

Let's take a look. 

![](https://media-exp1.licdn.com/dms/image/C4E22AQEsFG4xv2YrZw/feedshare-shrink_2048_1536/0/1664379742977?e=1667433600&v=beta&t=Pa3uvyEL_BtB5VjNUf5VsgScjmgFINNMe0TQU9XTXBo)

CAP theorem: one of the most misunderstood terms

The CAP theorem is one of the most famous terms in computer science, but I bet different developers have different understandings. Let’s examine what it is and why it can be confusing. 

CAP theorem states that a distributed system can't provide more than two of these three guarantees simultaneously.

𝐂𝐨𝐧𝐬𝐢𝐬𝐭𝐞𝐧𝐜𝐲: consistency means all clients see the same data at the same time no matter which node they connect to.

𝐀𝐯𝐚𝐢𝐥𝐚𝐛𝐢𝐥𝐢𝐭𝐲: availability means any client which requests data gets a response even if some of the nodes are down.

𝐏𝐚𝐫𝐭𝐢𝐭𝐢𝐨𝐧 𝐓𝐨𝐥𝐞𝐫𝐚𝐧𝐜𝐞: a partition indicates a communication break between two nodes. Partition tolerance means the system continues to operate despite network partitions. 

The “2 of 3” formulation can be useful, 𝐛𝐮𝐭 𝐭𝐡𝐢𝐬 𝐬𝐢𝐦𝐩𝐥𝐢𝐟𝐢𝐜𝐚𝐭𝐢𝐨𝐧 𝐜𝐨𝐮𝐥𝐝 𝐛𝐞 𝐦𝐢𝐬𝐥𝐞𝐚𝐝𝐢𝐧𝐠.

1. Picking a database is not easy. Justifying our choice purely based on the CAP theorem is not enough. For example, companies don't choose Cassandra for chat applications simply because it is an AP system. There is a list of good characteristics that make Cassandra a desirable option for storing chat messages. We need to dig deeper.

2. “CAP prohibits only a tiny part of the design space: perfect availability and consistency in the presence of partitions, which are rare”. Quoted from the paper: CAP Twelve Years Later: How the “Rules” Have Changed.

3. The theorem is about 100% availability and consistency. A more realistic discussion would be the trade-offs between latency and consistency when there is no network partition. See PACELC theorem for more details.

𝐈𝐬 𝐭𝐡𝐞 𝐂𝐀𝐏 𝐭𝐡𝐞𝐨𝐫𝐞𝐦 𝐚𝐜𝐭𝐮𝐚𝐥𝐥𝐲 𝐮𝐬𝐞𝐟𝐮𝐥?
I think it is still useful as it opens our minds to a set of tradeoff discussions, but it is only part of the story. We need to dig deeper when picking the right database.

---

Thanks. Unsurprisingly, this article was not written by me but [Alex Xu](https://www.linkedin.com/in/alexxubyte/) at [ByteByteGo](https://bytebytego.com/) .
