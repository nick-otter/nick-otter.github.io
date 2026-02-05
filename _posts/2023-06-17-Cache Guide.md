---
layout: post
title:  "Cache Guide"
tags: system-design
---

# An Introduction to Cache

| [Alex Xu](https://www.linkedin.com/in/alexxubyte/) | ["A Crash Course in Caching"](https://blog.bytebytego.com/p/a-crash-course-in-caching-part-1) |

Let's check it out.

# Introduction
Caching is a fundamental technique in computing that enables quick retrieval of frequently accessed data. A study conducted by Amazon found that increasing page load time by just 100 milliseconds costs 1% in sales. By storing frequently accessed data in faster storage, usually in memory, caching improves data retrieval speed and improves overall system performance. This highlights the significant impact caching can have on the user experience and ultimately on business success.

The following is a list of the topics we will cover in this series.

![](/assets/cache-overview.png)


What is a cache
Definition: according to Wikipedia: “In computing, a cache is a hardware or software component that stores data so that future requests for that data can be served faster; the data stored in a cache might be the result of an earlier computation or a copy of data stored elsewhere”.

Let's look at a concrete example.

When a client/server request is made without caching, the client sends a request to the service, which then retrieves data from storage and sends it back to the client. However, retrieving data from storage can become slow and overloaded with high volumes of traffic.

![](/assets/cache-1.png)


Adding a cache to the system can help overcome these limitations. When data is requested, the service first checks the cache for the data. If the data is present in the cache, it is quickly returned to the service. If the data is not found in the cache, the service retrieves it from storage, stores it in the cache, and then responds to the client. This allows for faster retrieval of frequently accessed data since cache usually stores data in memory in a data structure optimized for fast access.

![](/assets/cache-2.png)

Cache systems and storage systems differ in several ways. First, cache systems store frequently accessed data in memory. Storage systems store data on disks. Since memory is more expensive than disk, cache systems typically have much smaller data capacity than storage systems. This means that cache systems can store only a subset of the total data set.

Second, the data stored in cache systems are not designed for long-term data persistence and durability. Cache systems are used to improve performance. They do not provide durable data storage. In contrast, storage systems are designed to provide long-term data persistence and durability, ensuring that data is always available when needed.

Finally, cache systems are optimized for supporting heavy traffic and high concurrency. By storing data in memory, cache servers can respond quickly to incoming requests, which is crucial for high-traffic websites or applications. Storage systems, on the other hand, are better suited for durably storing and managing large amounts of data.

If the data that is requested is available in the cache, it is known as a *cache hit*, otherwise, it is known as a *cache miss*. Because accessing data from the cache is significantly faster than accessing it from storage, the system overall operates more efficiently when there are more cache hits. The effectiveness of a cache is measured by the cache hit ratio, which is the number of cache hits divided by the number of cache requests. Higher ratios indicate better overall performance.

# Where is caching used
While the previous example showcased caching in client/server computing, it is worth noting that caching is extensively used in modern computer and software systems.

![](/assets/cache-3.jpg)

Modern computers utilize multiple levels of cache, including the L1, L2, and L3 caches, to provide fast access to frequently used data for the CPU.

The Memory Management Unit (MMU) is responsible for mapping virtual memory addresses to physical memory addresses. The MMU contains a specialized cache called the Translation Lookaside Buffer (TLB), which stores the most recently used address translations to speed up the address translation process.

The operating system employs a page cache in the main memory to enhance overall system performance. The page cache stores frequently accessed data pages and reduces the number of disk accesses, which can slow down the system.

By utilizing these different levels of cache, modern computers can improve their overall performance and efficiency.

In software systems, caching plays a crucial role in enhancing performance and reducing network latency.

Browsers use a cache to store frequently accessed website images, data, and documents, resulting in faster load times and a smoother browsing experience. This cache is commonly known as the browser cache.

Content Delivery Networks (CDNs) are another form of caching used to deliver static resources such as images, videos, CSS files, and other multimedia content. CDNs are a geographically distributed network of proxy servers that work together to deliver content from the nearest server to the user's location. This significantly reduces the time taken to access the content, resulting in a faster-loading website.

Caching in databases is essential for improving performance and reducing execution overhead. Some typical caches found in databases include the buffer cache, result cache, query cache, metadata cache, and session cache. These caches store frequently accessed data blocks, query results, metadata, and session-specific information in memory to reduce disk reads and query execution time, resulting in faster query response times and a better user experience.

Two widely used caching systems are Memcached and Redis. They are open-source, high-performance, distributed caching systems that can be used to store and retrieve data quickly.

Real-world use cases for caching include storing historical emails locally to avoid repeatedly pulling the same data from the server, caching popular tweets on social media websites like Twitter, and preloading and caching product information for flash sale systems to prevent excessive database pressure.

Caching is an effective solution for scenarios where data changes infrequently, the same data is accessed multiple times, the same output is produced repeatedly, or the results of time-consuming queries or calculations are worth caching.

While there are many different types of caches in hardware and software, this article will focus on the general caching use cases in system design, exploring its principles, strategies, and problems.


# System-on-Chip (SOC) Cache Hierarchy

Fundamental to Cache is knowing the System-On-Chip (SOC). This document by Alex Xu provides a nice example, I've added a more practical picture also.

| Alex Xu | ["Which latency numbers should you know?"](https://blog.bytebytego.com/p/ep22-latency-numbers-you-should-know) |

Let's check it out. 

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fd363c7d8-6571-48ea-bb05-6e4a09d48f45_1974x1096.jpeg)

Simpler version: 

![](/assets/cache-soc-hierarchy.jpeg)


* **L1 and L2 CPU caches**: 1 ns, 10 ns
E.g.: They are usually built onto the microprocessor chip. Unless you work with hardware directly, you probably don’t need to worry about them.

* **RAM access**: 100 ns
E.g.: It takes around 100 ns to read data from memory. Redis is an in-memory data store, so it takes about 100 ns to read data from Redis.

* **Send 1K bytes over 1 Gbps network**: 10 us
E.g.: It takes around 10 us to send 1KB of data from Memcached through the network.

* **Read from SSD**: 100 us
E.g.: RocksDB is a disk-based K/V store, so the read latency is around 100 us on SSD.

* **Database insert operation**: 1 ms.
E.g.: Postgresql commit might take 1ms. The database needs to store the data, create the index, and flush logs. All these actions take time.

* **Send packet CA->Netherlands->CA**: 100 ms
E.g.: If we have a long-distance Zoom call, the latency might be around 100 ms.

* **Retry/refresh internal**: 1-10s
E.g: In a monitoring system, the refresh interval is usually set to 5~10 seconds (default value on Grafana).

Notes
* 1 ns = 10^-9 seconds
* 1 us = 10^-6 seconds = 1,000 ns
* 1 ms = 10^-3 seconds = 1,000 us = 1,000,000 ns

An updated conversation about Latency times can be found here: https://gist.github.com/jboner/2841832

And a nice comprehension using solar system references is here: 

| Operation |	Time in Nano Seconds |	Astronomical Unit of Weight |
| ------ | ------ | ----- |
| L1 cache reference |	0.5 ns |	1/2 Earth or Five times Mars |
| Branch mispredict |	5 ns	| 5 Earths |
| L2 cache reference |	7 ns |	7 Earths |
| Mutex lock/unlock |	25 ns |	Roughly [Uranus +Neptune] |
| Main memory reference |	100 ns	| Roughly Saturn + 5 Earths |
| Compress 1K bytes with Zippy |	3,000 ns |	10 Jupiters |
| Send 1K bytes over 1 Gbps network	| 10,000 ns	| 20 Times All the Planets of the Solar System |
| Read 4K randomly from SSD* |	150,000 ns |	1.6 times Red Dwarf Wolf 359 |
| Read 1 MB sequentially from memory |	250,000 ns |	Quarter of the Sun |
| Round trip within same datacenter |	500,000 ns |	Half of the Mass of Sun |
| Read 1 MB sequentially from SSD* |	1,000,000 ns |	Sun |
| Disk seek	| 10,000,000 ns |	10 Suns |
| Read 1 MB sequentially from disk |	20,000,000 ns |	Red Giant R136a2 |
| Send packet CA->Netherlands->CA	| 150,000,000 ns |	An Intermediate Sized Black Hole |


# Distributed Cache

| Alex Xu | ["A Crash Course in Caching"](https://blog.bytebytego.com/p/a-crash-course-in-caching-part-1) |

# Introduction To Distributed Cache
A distributed cache stores frequently accessed data in memory across multiple nodes. The cached data is partitioned across many nodes, with each node only storing a portion of the cached data. The nodes store data as key-value pairs, where each key is deterministically assigned to a specific partition or shard. When a client requests data, the cache system retrieves the data from the appropriate node, reducing the load on the backing storage.

There are different sharding strategies, including modulus, range-based and consistent hashing.

# Modulus Sharding
Modulus sharding involves assigning a key to a shard based on the hash value of the key modulo the total number of shards. Although this strategy is simple, it can result in many cache misses when the number of shards is increased or decreased. This is because most of the keys will be redistributed to different shards when the pool is resized.

![](/assets/distributed-cache-1.jpg)

# Range-based Sharding
Range-based sharding assigns keys to specific shards based on predefined key ranges. With this approach, the system can divide the key space into specific ranges and then map each range to a particular shard. Range-based sharding can be useful for certain business scenarios where data is naturally grouped or partitioned in specific ranges, such as geolocation-based data or data related to specific customer segments.

![](/assets/distributed-cache-2.jpg)

However, this approach can also be challenging to scale because the number of shards is predefined and cannot be easily changed. Changing the number of shards requires redefining the key ranges and remapping the data.

# Consistent hashing
Consistent hashing is a widely-used sharding strategy that provides better load balancing and fault tolerance than other sharding methods. With consistent hashing, the keys and the nodes are both mapped to a fixed-size ring, using a hash function to assign each key and node a position on the ring.

![](/assets/distributed-cache-3.jpg)

When a key is requested, the system uses the same hash function to map the key to a position on the ring. The system then traverses the ring clockwise from that position until it reaches the first node. That node is responsible for storing the data associated with the key. Adding or removing nodes from the system only requires remapping the keys that were previously stored on the affected nodes, rather than redistributing all the keys, making it easy to change the number of shards with a limited amount of data rehashed. Refer to the reference material to learn more about consistent hashing.

# Cache Strategies
In this section, we will provide an in-depth analysis of various cache strategies, their characteristics, and suitable use cases.

Cache strategies can be classified by the way they handle reading or writing data:

1. Read Strategies: Cache-Aside and Read-Through<br>
2. Write Strategies: Write-Around, Write-Through, and Write-Back

# Read Strategies
## Cache-aside
Cache-aside, also known as lazy loading, is a popular cache strategy where the application directly communicates with both the cache and storage systems. It follows a specific workflow for reading data:

1 .The application requests a key from the cache.<br>
2. If the key is found in the cache (cache hit), the data is returned to the application.<br>
3. If the key is not found in the cache (cache miss), the application proceeds to request the key from storage.<br>
4. The storage returns the data to the application.<br>

The application writes the key and corresponding data into the cache for future reads.

![](/assets/distributed-cache-4.jpg)

Cache-aside is versatile, as it can accommodate various use cases and adapts well to read-heavy workloads.

Pros:

* The system can tolerate cache failures, as it can still read from the storage.

* The data model in the cache can differ from that in the storage, providing flexibility for a variety of use cases.

Cons:

* The application must manage both cache and storage, complicating the code.

* Ensuring data consistency is challenging due to the lack of atomic operations on cache and storage.

When using cache-aside, it is crucial to consider other potential data consistency issues. For instance, if a piece of data is written to the cache, and the value in the storage is updated afterward, the application can only read the stale value from the cache until it is evicted. One solution to this problem is to set an acceptable Time-To-Live (TTL) for each cached record, ensuring that stale data becomes invalid after a certain period. For more stringent data freshness requirements, the application can combine cache-aside with one of the write strategies that are discussed below.

# Read-through
The read-through strategy is another common caching approach where the cache serves as an intermediary between the application and the storage system, handling all read requests. This strategy simplifies the application's role by delegating data retrieval responsibilities to the cache. Read-through is particularly suitable for read-heavy workloads and is commonly employed in cache libraries and some stand-alone cache providers. The basic workflow for read-through follows these steps:

1. The application requests to read a key from the cache.<br>

2. If the key is found in the cache (cache hit), the data is returned to the application.<br>

3. If the key is not found in the cache (cache miss), the cache requests the key from the storage.<br>

4. The cache retrieves the data from storage, writes the key and associated data into the cache, and returns the data to the application.<br>

![](/assets/distributed-cache-5.png)

---


# Stampede

| Alex Xu | [Alex Xu](https://www.linkedin.com/in/alexxubyte/) |

Caching is awesome but it doesn’t come without a cost, just like many things in life.

One of the issues is *Cache Stampede*. Please correct me if this is not the right term. It refers to the scenario where data to fetch doesn't exist in the database and the data isn’t cached either. So every request hits the database eventually, defeating the purpose of using a cache. If a malicious user initiates lots of queries with such keys, the database can easily be overloaded.

The diagram below illustrates the process.

# Stampede Diagram

![](/assets/cache_stampede.jpeg)

# Invalidation

| Arsan Ahmad | [Arslan Ahmad](https://www.linkedin.com/in/arslanahmad/)|

Cache invalidation is the process of removing or updating cached data when it becomes outdated or no longer accurate. "Purge," "refresh," and "ban" are commonly used cache invalidation methods that are frequently used in the application cache, content delivery networks (CDNs), and web proxies. Here's a brief description of a few famous cache invalidation methods:

# Cache Invalidation Methods

🔹𝗣𝘂𝗿𝗴𝗲: The purge method removes cached content for a specific object, URL, or a set of URLs. It's typically used when there is an update or change to the content and the cached version is no longer valid. When a purge request is received, the cached content is immediately removed, and the next request for the content will be served directly from the origin server.

🔹𝗥𝗲𝗳𝗿𝗲𝘀𝗵: Fetches requested content from the origin server, even if cached content is available. When a refresh request is received, the cached content is updated with the latest version from the origin server, ensuring that the content is up-to-date. Unlike a purge, a refresh request doesn't remove the existing cached content; instead, it updates it with the latest version.

🔹𝗕𝗮𝗻: The ban method invalidates cached content based on specific criteria, such as a URL pattern or header. When a ban request is received, any cached content that matches the specified criteria is immediately removed, and subsequent requests for the content will be served directly from the origin server.

🔹𝗧𝗶𝗺𝗲-𝘁𝗼-𝗹𝗶𝘃𝗲 (𝗧𝗧𝗟) 𝗲𝘅𝗽𝗶𝗿𝗮𝘁𝗶𝗼𝗻: This method involves setting a time-to-live value for cached content, after which the content is considered stale and must be refreshed. When a request is received for the content, the cache checks the time-to-live value and serves the cached content only if the value hasn't expired. If the value has expired, the cache fetches the latest version of the content from the origin server and caches it.

🔹𝗦𝘁𝗮𝗹𝗲-𝘄𝗵𝗶𝗹𝗲-𝗿𝗲𝘃𝗮𝗹𝗶𝗱𝗮𝘁𝗲: This method is used in web browsers and CDNs to serve stale content from the cache while the content is being updated in the background. When a request is received for a piece of content, the cached version is immediately served to the user, and an asynchronous request is made to the origin server to fetch the latest version of the content. Once the latest version is available, the cached version is updated. This method ensures that the user is always served content quickly, even if the cached version is slightly outdated.

![](/assets/cache_invalidation.jpeg)


# Managing Operational Challenges in Caching

| [Alex Xu](https://www.linkedin.com/in/alexxubyte/) |["A Crash Course in Caching"](https://blog.bytebytego.com/p/a-crash-course-in-caching-part-1) |

# Introduction

When implementing caching systems, it is crucial to not only choose the best cache strategies but also address the operational challenges that may arise. The following section dives into common caching problems by category and provides insight into how to tackle these challenges effectively.

# Reliability Challenges
Cache reliability is important for maintaining stable system performance. Common reliability issues include cache avalanche, cache stampede, and cache penetration.

## Mitigating cache avalanche and cache stampede
Caching systems play a crucial role in protecting databases by maintaining high cache hit ratios and reducing traffic to storage systems. Under normal conditions, most read requests result in cache hits, with only a small portion leading to cache misses. By design, the cache handles the majority of the traffic, and the storage system experiences significantly less load.

Cache avalanche and cache stampede are related but distinct phenomena that can occur in large scale systems, causing significant performance degradation or even system-wide outages. Cache avalanche happens when multiple or all cache entries expire simultaneously or within a short time window, leading to a sudden surge in requests to the underlying data store.

![](/assets/cache-challenge-1.png)

Cache stampede, also known as thundering herd, occurs in large scale systems when a sudden influx of requests overwhelms the system, causing performance degradation or even system-wide outages. This can happen due to various reasons, such as cache misses on popular items, a sudden spike in user traffic, or service restarts after maintenance.

![](/assets/cache-challenge-2.png)

When a cache or part of it fails, a large number of read requests result in cache misses, causing a massive influx of traffic to the storage system. This sudden increase in traffic can significantly reduce storage performance or even lead to system crashes. In this scenario, the failure of the cache serves as a trigger that combines elements of both cache avalanche and cache stampede, resulting in a high-stress situation for the storage system.

Traffic peaks, such as during high-traffic events like Black Friday sales on e-commerce sites in the United States, can also trigger these phenomena. A single cache node might crash due to the increased demand for popular keys, causing these keys to be rehashed to another node, which then crashes as well, eventually leading to the collapse of the entire cache system.

To maintain the stability and efficiency of large scale systems, it is essential to implement strategies that mitigate the risks of both cache avalanche and cache stampede. There are  several techniques to help prevent and alleviate the impact of these events.

## Staggered expiration
For cache avalanche prevention, use staggered expiration by combining a base time-to-live (TTL) value with a random delta. This approach spreads out the expiration times of cached entries, reducing the likelihood of many items expiring at once.

![](/assets/cache-challenge-3.png)

## Consistent hashing
Consistent hashing can be used to distribute cache entries across multiple cache servers evenly. This technique reduces the impact of cache avalanche and cache stampede by sharing the load among the remaining servers and preventing a single point of failure.

## Circuit breakers
Implementing circuit breakers in the system can help prevent cache avalanche and cache stampede from escalating into more severe issues. Circuit breakers monitor the system's health and prevent overloading by stopping excessive requests to the cache and data store if the system is under high stress.

## Request rate limiting and load shedding
Employing request rate limiting and load shedding can specifically address cache stampede by controlling the rate at which requests are processed and preventing the system from being overwhelmed. These techniques can be applied on a per-user, per-client, or system-wide basis to maintain stability during high-load situations.

# Addressing Cache Penetration Issues
Cache penetration occurs when an application attempts to access non-existent data, bypassing the cache and leading to potential performance issues. When an application requests a non-existent key, it results in a cache miss, followed by a request to the storage system, which also returns an empty result. This bypassing of the cache to reach the backing storage is referred to as cache penetration. If a large number of non-existent key requests occur, it can impact the performance of the storage layer and destabilize the overall system.

![](/assets/cache-challenge-4.png)

A common example of cache penetration occurs when numerous users try to query non-existent orders on a website within a short period. Large websites typically implement protective measures to mitigate this issue.

To mitigate this, store a placeholder value in the cache to represent non-existent data. Subsequent requests for the same non-existent data will hit the cache instead of the data store. Set an appropriate TTL for these placeholder entries to prevent them from occupying cache space indefinitely.

![](/assets/cache-challenge-5.jpg)

While this method is simple and effective, it can consume significant cache resources if the application frequently queries a large number of non-existent keys.

Another solution involves using a bloom filter, a space-efficient, probabilistic data structure that tests whether an element belongs to a set. By using a bloom filter, the system can quickly identify non-existent data, reducing cache penetration and unnecessary data store requests.

When a record is added to storage, its key is also recorded in a bloom filter.  When fetching a record, the application first checks whether the key exists in the bloom filter. If the key is absent, it won't exist in the cache or storage either, and the application can return a null value directly. If the key is present in the bloom filter, the application proceeds to read the key from the cache and storage as usual. Since a bloom filter sometimes returns a false positive, a small number of the cache reads will result in a cache miss.

![](/assets/cache-challenge-6.jpg)

The challenge of using a bloom filter lies in its capacity limitations. For instance, 1 billion keys with a 1% false positive rate would require approximately 1.2 GB of capacity. This solution is best suited for smaller data sets.

# Traffic Pattern Challenges
## Hot key problem
The hot key problem occurs when high traffic on a single key results in excessive traffic pressure on the cache node, potentially leading to performance issues.

In a distributed cache system, keys are shared across different nodes based on a sharding strategy. Traffic for each key can vary greatly, with some keys experiencing exceptionally high traffic. As illustrated in the diagram below, substantial traffic for keys like "key12" and "key15" could potentially exceed the resource capacity of node 1.

For instance, when tweets are distributed in the cache system by their IDs, and a few of them become popular quickly, the corresponding cache node experiences increased traffic, possibly surpassing its resource capabilities. Hot key issues can arise in various scenarios, such as operational emergencies, festivals, sports games, flash sales, etc.

![](/assets/cache-challenge-7.jpg)

## Hot key in distributed cache
To address this problem, two steps are necessary:

1. Identify the hot keys.<br>

2. Implement special measures for these keys to reduce traffic pressure.<br>

In step 1, for predictable events like important holidays or online promotions, it is possible to evaluate potential hot keys beforehand. However, for emergencies, advance evaluation is not feasible. One solution is to conduct real-time traffic analysis to promptly detect emerging hot keys.

In step 2, there are several potential solutions:

* Distribute traffic pressure across the entire cache system. As illustrated in the diagram below, split "key12" into "key12-1", "key12-2", up to "key12-N", and distribute these N keys across multiple nodes. When an application requests a hot key, it randomly selects one of the suffixed keys, enabling traffic to be shared across many nodes and preventing a single node from becoming overloaded.

![](/assets/cache-challenge-8.jpg)

If numerous hot keys exist, real-time monitoring can enable the cache system to expand quickly, reducing the traffic impact.

As a last resort, applications can store hot keys in a local cache, decreasing traffic to the remote cache system. This approach alleviates the pressure on the cache nodes and helps maintain overall system performance.

# Large key problem
When the value size of a key is significantly large, it can result in access timeouts, leading to the large key problem.

The impact of the large key problem is more severe than it may initially seem:

* Frequent access to large keys can consume significant network bandwidth, leading to slow lookup performance.

* Any partial update of a large key will result in the modification of the entire value, further contributing to performance issues.

* If a large key becomes invalid or is evicted from the cache, reloading it from storage can be time-consuming, leading to additional slow query performance.

![](/assets/cache-challenge-9.png)