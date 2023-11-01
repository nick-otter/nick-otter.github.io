---
layout: post
title:  "Latency Numbers"
categories: latency
---

# Latency Numbers, L1 Cache To Retry
{: style="text-align: center"}

From Alex Xu at ByteByteGo.

[Alex Xu](https://www.linkedin.com/in/alexxubyte/) at [ByteByteGo](https://bytebytego.com/) recently published ["Which latency numbers should you know?"](https://blog.bytebytego.com/p/ep22-latency-numbers-you-should-know).

Let's check it out. 

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fd363c7d8-6571-48ea-bb05-6e4a09d48f45_1974x1096.jpeg)

(These are based on some online benchmarks e.g. Jeff Dean’s latency numbers + some other sources).

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

---

Thanks. Unsurprisingly, this article was not written by me but [Alex Xu](https://www.linkedin.com/in/alexxubyte/) at [ByteByteGo](https://bytebytego.com/) .
