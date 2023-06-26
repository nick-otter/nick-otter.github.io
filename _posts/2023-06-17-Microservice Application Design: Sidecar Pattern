---
layout: post
title:  "Microservice Application Design: Sidecar Pattern"
categories: microservices
---

# Microservice Application Design: Sidecar Pattern
{: style="text-align: center"}

From [Emre Savci](https://itnext.io/application-architecture-for-microservices-sidecar-pattern-c5c0074e8f1d).

Let’s examine together how we use sidecar to solve cross-cutting concerns such as authorization, caching, configuration secret management, and observability.

# Cross-Cutting Concerns

Some of the requirements outside of the application’s business code are needed in the different layers of an application.

For example logging, configuration, caching, authorization, observability…

![](/assets/micro_app.png)

# Traditional Development

The traditional way is to code and use all this tooling within our development service. If we have different services written in the same language, we can share those codes between services and use them as packages/libraries between services.

![](/assets/micro_traddev.png)

So what is a bad scenario?

In an environment where services are written in more than one language, these requirements had to be reimplemented with each language.

Imagine that you (or your teams) are writing the same functionality over and over with different languages…

![](/assets/micro_multilang.png)

Is there a solution in this case instead of writing the same code?

The first thing that comes to mind is to write separate API services for those requirements.

So is this a correct solution?

What problems can we encounter?

![](/assets/micro_shareservices.png)

How do scale these shared services when the number of consumer services increases?

Moving the application code to another service that accessing over the network will cause the network latency(delay) to our application. It will slow down our application.

So, what would you do if you do not want to rewrite the same stories in different languages, or face network latencies?

Answer: We will use to Sidecar pattern.

# Sidecar Pattern

![](/assets/micro_apppatterns.png)

![](/assets/kube_sidecar.png)

So what is this Sidecar?

We can run multiple containers inside our pods running on Kubernetes. We can call the containers running beside our main application container as sidecars.

These additional features can develop in a single language and then we can “inject” them into our applications. We develop our sidecars with the Go language and inject them into microservices.

This “inject” operation is performed by Kubernetes on the basis of a concept named Dynamic Admission Webhook. I will leave the source link at the end of the article for details.

![](/assets/kuber_sidecarinject.png)

When a pod scales up, the additional sidecars also go with it.

In addition, with the network interface created specifically for the pod shared across sidecars, network latency can be ignored because it is over localhost.

![](/assets/kube_sidecarinjectscale.png)

What else can be done with the sidecars?

Pod network configurations
Pod network requests/responses can be interrupted/manipulated
Service mesh can be made
Microservice runtime implementation can be done
For example, Istio Service Mesh can work as sidecar. The pod’s entire network control is taken over by the incoming istio-proxy as a sidecar and can manipulate incoming and outgoing requests/responses.

![](/assets/micro_istio.png)

You can write various network rules. When the whole network passes through the istio-proxy sidecar, there are metric data for all network processes and we can visualize microservice inter-service calls.

![](/assets/micro_istiointerservice.png)

---