---
layout: post
title:  "Troubleshooting Kubernetes Deployments"
categories: kubernetes_cheatsheets
---

# A Visual Guide On Troubleshooting Kubernetes Deployments
{: style="text-align: center"}

From Daniele Polencic at Learn k8s.

[Daniele Polencic](https://www.linkedin.com/in/danielepolencic/) at [Learn k8s](https://learnk8s.io/) recently published ["A visual guide on troubleshooting Kubernetes deployments"](https://learnk8s.io/troubleshooting-deployments).
It's the most concise breakdown for troubleshooting k8s I've seen. I recommend reading it. 

The main piece is this flow chart. It's well worth reading through too [here](https://learnk8s.io/troubleshooting-deployments).
![](https://learnk8s.io/a/fae60444184ca7bd8c3698d866c24617.png)

And here's the excerpt:

When you wish to deploy an application in Kubernetes, you usually define three components:

* A Deployment — which is a recipe for creating copies of your application called Pods.
* Service — an internal load balancer that routes the traffic to Pods.
* An Ingress — a description of how the traffic should flow from outside the cluster to your Service.

Here's a quick visual recap.

![](https://learnk8s.io/a/92543837cbecdd1189ee0a6d68fa9434.svg)

In Kubernetes your applications are exposed through two layers of load balancers: internal and external.

Assuming you wish to deploy a simple Hello World application, the YAML for such application should look similar to this:

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


The definition is quite long, and it's easy to overlook how the components relate to each other.

For example:

* When should you use port 80 and when port 8080?
* Should you create a new port for every Service so that they don't clash?
* Do label names matter? Should it be the same everywhere?
* Before focusing on the debugging, let's recap how the three components link to each other._

Let's start with Deployment and Service.

## Connecting Deployment and Service
The surprising news is that Service and Deployment aren't connected at all.

Instead, the Service points to the Pods directly and skips the Deployment altogether.

So what you should pay attention to is how the Pods and the Services are related to each other.

You should remember three things:

* The Service selector should match at least one Pod's label.
* The Service's targetPort should match the containerPort of the Pod.
* The Service port can be any number. Multiple Services can use the same port because they have different IP addresses assigned.

The following diagram summarises how to connect the ports:
![](/assets/k8s-1.jpg)

If you look at the YAML, the labels and `ports`/`targetPort` should match:
![](/assets/k8s-2.kpg)

What about the `track: canary` label at the top of the Deployment?

Should that match too?

That label belongs to the deployment, and it's not used by the Service's selector to route traffic.

In other words, you can safely remove it or assign it a different value.

And what about the `matchLabels` selector?

It always has to match the Pod's labels and it's used by the Deployment to track the Pods.

Assuming that you made the correct change, how do you test it?

You can check if the Pods have the right label with the following command:
```
kubectl get pods --show-labels
```
Or if you have Pods belonging to several applications:
```
kubectl get pods --selector any-name=my-app --show-labels
```
Still having issues?

You can also connect to the Pod!

You can use the `port-forward` command in kubectl to connect to the Service and test the connection.

```
kubectl port-forward service/<service name> 3000:80
```
Where:

* `service/<service name>` is the name of the service — in the current YAML is "my-service".
* 3000 is the port that you wish to open on your computer.
* 80 is the port exposed by the Service in the port field.

If you can connect, the setup is correct.

If you can't, you most likely misplaced a label or the port doesn't match.

...

Enjoy!

---

The entire content of this article is thanks to [Daniele Polencic](https://www.linkedin.com/in/danielepolencic/) at [Learn K8s](https://learnk8s.io/), ["A visual guide on troubleshooting Kubernetes deployments"](https://learnk8s.io/troubleshooting-deployments).
