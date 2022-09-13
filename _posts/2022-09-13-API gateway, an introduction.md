---
layout: post
title:  "An API gateway introduction from Alex Xu at ByteByteGo"
categories: solutions apigateway
---

# An API Gateway introduction from Alex Xu at ByteByteGo
{: style="text-align: center"}

[Alex Xu](https://www.linkedin.com/in/alexxubyte/) at [ByteByteGo](https://bytebytego.com/) recently published an ["API gateway introduction"](https://blog.bytebytego.com/p/ep23-how-to-choose-the-right-database).

Here is the full post. 

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fec471df9-1798-4efb-86e8-b1db33e3a123_1502x2222.png)

```
Step 1 - The client sends an HTTP request to the API gateway.

Step 2 - The API gateway parses and validates the attributes in the HTTP request.

Step 3 - The API gateway performs allow-list/deny-list checks.

Step 4 - The API gateway talks to an identity provider for authentication and authorization.

Step 5 - The rate limiting rules are applied to the request. If it is over the limit, the request is rejected.

Steps 6 and 7 - Now that the request has passed basic checks, the API gateway finds the relevant service to route to by path matching.

Step 8 - The API gateway transforms the request into the appropriate protocol and sends it to backend microservices.

Steps 9-12: The API gateway can handle errors properly, and deals with faults if the error takes a longer time to recover (circuit break). It can also leverage ELK (Elastic-Logstash-Kibana) stack for logging and monitoring. We sometimes cache data in the API gateway.
```

---

Thanks. Unsurprisingly, this article was not written by me but [Alex Xu](https://www.linkedin.com/in/alexxubyte/) at [ByteByteGo](https://bytebytego.com/) .
