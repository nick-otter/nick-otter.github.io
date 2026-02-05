---
layout: post
title:  "Microservices: Fault Tolerant Design"
categories: microservices
---

| Leo Coelho | [Leo Coelho](https://www.linkedin.com/in/leo-coelho/) |


Some patterns to ensure resilience and fault tolerance of individual services:

# Retry Pattern

1. Retry Pattern: It handles transient failures in communication between microservices. When a request fails due to network issues or temporary unavailability of a service, the pattern retries the request automatically with a back-off strategy, giving the service a chance to recover.

# Timeout Pattern

2. Timeout Pattern: It defines the maximum duration for a request to be processed. Timeouts help protect against resource exhaustion and keep the system responsive.

# Rate Limiting

3. Rate Limiting: It controls the number of requests a client or service can make within a specific time frame. Rate limiting helps protect services from excessive or abusive traffic, ensuring fair resource allocation and preventing service degradation or disruption.

# Backpressure

4. Backpressure: It regulates the flow of data or requests between microservices when one service can't process incoming requests at the same rate. Instead of overwhelming a slow or resource-constrained service, it propagates a signal upstream to slow down or stop sending additional requests.

# Circuit Breaker

5. Circuit Breaker Pattern: It protects your system from cascading failures and provides fault tolerance when dealing with remote service dependencies. It monitors the availability of a service and, if it detects repeated failures or timeouts, opens the circuit, preventing further requests from being sent. It allows the system to gracefully handle the failure and provides fallback mechanisms, such as returning cached data or displaying a user-friendly error message. When the faulty service recovers, the circuit breaker gradually closes, allowing requests to flow again.

Note that we can combine patterns to tackle different aspects of resiliency and fault tolerance. For instance:

Retry and Circuit Breaker: When a request fails, the Retry Pattern can automatically retry the request with a back-off strategy, giving the service a chance to recover. However, if the failures persist or reach a threshold, the Circuit Breaker Pattern can open the circuit and stop sending requests to prevent further damage.
