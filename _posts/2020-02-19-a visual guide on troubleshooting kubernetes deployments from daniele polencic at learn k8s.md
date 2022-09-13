---
layout: post
title:  "A visual guide on troubleshooting Kubernetes deployments from Daniele Polencic at Learn k8s"
categories: kubernetes general
---

# A visual guide on troubleshooting Kubernetes deployments from Daniele Polencic at Learn k8s
{: style="text-align: center"}


[Daniele Polencic](https://www.linkedin.com/in/danielepolencic/) at [Learn k8s](https://learnk8s.io/) recently published ["A visual guide on troubleshooting Kubernetes deployments"](https://learnk8s.io/troubleshooting-deployments).
It's the most concise breakdown for troubleshooting k8s I've seen. I recommend reading it. 

The main piece is this flow chart. It's well worth reading through too [here](https://learnk8s.io/troubleshooting-deployments).
![](https://learnk8s.io/a/fae60444184ca7bd8c3698d866c24617.png)

And here's an excerpt:

_When you wish to deploy an application in Kubernetes, you usually define three components:_

_A Deployment — which is a recipe for creating copies of your application called Pods.
A Service — an internal load balancer that routes the traffic to Pods.
An Ingress — a description of how the traffic should flow from outside the cluster to your Service.
Here's a quick visual recap._

![](https://learnk8s.io/a/92543837cbecdd1189ee0a6d68fa9434.svg)

_In Kubernetes your applications are exposed through two layers of load balancers: internal and external._

_Assuming you wish to deploy a simple Hello World application, the YAML for such application should look similar to this:_

```yaml
hello-world.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
  labels:
    track: canary
spec:
  selector:
    matchLabels:
      any-name: my-app
  template:
    metadata:
      labels:
        any-name: my-app
    spec:
      containers:
        - name: cont1
          image: learnk8s/app:1.0.0
          ports:
            - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  ports:
    - port: 80
      targetPort: 8080
  selector:
    name: app
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - http:
      paths:
      - backend:
          service:
            name: my-service
            port:
              number: 80
        path: /
        pathType: Prefix
```


_The definition is quite long, and it's easy to overlook how the components relate to each other._

_For example:_

_When should you use port 80 and when port 8080?
Should you create a new port for every Service so that they don't clash?
Do label names matter? Should it be the same everywhere?
Before focusing on the debugging, let's recap how the three components link to each other._

_Let's start with Deployment and Service._

_Connecting Deployment and Service_
_The surprising news is that Service and Deployment aren't connected at all._

_Instead, the Service points to the Pods directly and skips the Deployment altogether._

_So what you should pay attention to is how the Pods and the Services are related to each other._

_You should remember three things:_

_The Service selector should match at least one Pod's label.
The Service's targetPort should match the containerPort of the Pod.
The Service port can be any number. Multiple Services can use the same port because they have different IP addresses assigned.
The following diagram summarises how to connect the ports:_
...

Enjoy!

---

The entire content of this article is thanks to [Daniele Polencic](https://www.linkedin.com/in/danielepolencic/) at [Learn K8s](https://learnk8s.io/), ["A visual guide on troubleshooting Kubernetes deployments"](https://learnk8s.io/troubleshooting-deployments).
