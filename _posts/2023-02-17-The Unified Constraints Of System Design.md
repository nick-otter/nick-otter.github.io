---
layout: post
title:  "The Unified Constraints of System Design"
tags: system-design
---

# The Unified Constraints Of System Design
{: style="text-align: center"}

Rocky Bhatia | [The Unified Constraints Of System Design](https://www.linkedin.com/in/rocky-bhatia-a4801010?miniProfileUrn=urn%3Ali%3Afs_miniProfile%3AACoAAAIjFtgB476KvCJqtfrug8LN7y82m1bV2JQ&lipi=urn%3Ali%3Apage%3Ad_flagship3_detail_base%3BG9eywd%2FCQxGaWRR5y30kVA%3D%3D)


# Rocky Bhatia

A strong hold on System Design is a must to crack any good company ,No matter what type of engineer you are, knowing System Design will make you more well-rounded, and get you more success in your career.

Please remember the below Diagram, beautifully captured by Vahid, to get a strong hold on System Design.

1. Features

It’s always good to start with recapping the main features of the system you are trying to build. The prompt you get is typically vague on purpose–so reviewing the features is a good way to start.


2. Users

Next, think about the users of the system.
Are there different types? (admin, user, etc).
When do most users use the system, and how many people do we need to serve? How fast is our userbase growing?


3. Data Model

Once you have a good understanding of the features of the system, and the types of users you need to handle, you are ready to tackle the data model. Here, you should think about whether there's any particular reason why a NoSQL database would be better than a relational database for your use case


4. Geography & Latency

The next issue you may want to consider and possibly address is related to reducing latency, especially if your system is spread out across the globe.


5. Server Capacity

Next is the issue of the hardware capacity of the servers themselves, including the ones hosting your databases.
What are the CPU, RAM, and storage needs?

6. APIs & Security

If you're designing an external or internal API, there are several points to consider. Soap vs Rest Vs GraphQL

7. Availability / Microservices

If your system needs to have high availability (think five 9s: 99.999% availability per year), how could you ensure this?
What types of redundancies could you set up so that you’ve always got an available server in case one crashes?
If you’ve got microservices depending on each other.

8. Caching

To increase the speed of reads, you may take advantage of caching, which can be done on multiple levels / layers of your system.
What could you cache directly on the user’s device?
What would you cache just between a microservice and your database?

9. Proxies

This brings us to next topic: proxies! If availability is important (and it almost always is), you're probably going to have multiple instances of the same server, and you'll therefore need one or more load balancer(s), which is a type of reverse proxy.

10. Messaging

Finally, it might be worthwhile to consider any messaging paradigms and tools you might use, whether for your internal, server-to-server communication, or between end users and your servers.

Do you need to use some kind of messaging bus like Kafka or RabbitMQ, push/pull based ?

Above points will surely help you in a long term

![](/assets/sys_design.jpg.jpg)

---

Thanks. This was written by Nick Otter.
