---
layout: post
title:  "How to monitor Kubernetes with Prometheus"
categories: microservices kubernetes
---

# How to monitor Kubernetes with Prometheus
{: style="text-align: center"}

# Contents 

- [**Introduction**](#introduction)<br>
- [**Requirements**](#requirements)<br>
- [**Deploying Prometheus with Helm**](#deploying-prometheus-with-helm)<br>
   - [Helm values.yaml](#helm-values.yaml)<br>
- [**Exposing the web dashboards**](#exposing-the-web-dashboards)<br>
- [**Looking at targets in the Web UI**](#looking-at-targets-in-the-web-ui)<br>
- [**Exposing metrics for Prometheus**](#exposing-metrics-for-prometheus)<br>
   - [Configuring a metrics endpoint on a pod](#configuring-a-metrics-endpoint-on-a-pod)<br>
   - [Checking current deployment and creating a Service](#checking-current-deployment-and-creating-a-service)<br>
   - [Creating a ServiceMonitor resource](#creating-a-servicemonitor-resource)
<br><br>

# Introduction

Let's take a look at Prometheus as a monitoring solution for a simple cluster. Here's a nice overview from [Sysdig.com](https://sysdig.com/blog/kubernetes-monitoring-prometheus/). In this article we'll figure out how to deploy Prometheus using Helm, how to expose the web dashboards to outside of the cluster, take a quick look at the Prometheus web UI and how to expose a Traefik ingress resource to be polled by Prometheus. Nice.<br>

![](https://478h5m1yrfsa3bbe262u7muv-wpengine.netdna-ssl.com/wp-content/uploads/2018/08/prometheus_kubernetes_diagram_overview.png)

I don't like presenting images without info. So.. take a look over the image and talk it over. Only if you want to though.
<br><br>

# Requirements

| Kubernetes | `1.19.2` |
| Minikube | `1.13.1`|
| Helm | `3.4.0` |
| Prometheus | `kube-prometheus-stack-12.2.0`|
<br><br>

# Deploying Prometheus with Helm 

I used the chart [kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) for this (_it's the currently maintained chart at the time of writing this_ 20112020).

Let's deploy **`Prometheus`** under the name **`prometheus`** in the Kubernetes namespace **`monitoring`** using **`helm`**.
```
$ helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring
```
```
[minikube@control-plane helpers]$ kubectl get po -n monitoring
NAME                                                   READY   STATUS    RESTARTS   AGE
prometheus-grafana-58cf5655dc-sq8rs                    2/2     Running   0          75m
prometheus-kube-prometheus-operator-648b6f79cd-6v2f5   1/1     Running   0          75m
prometheus-kube-state-metrics-95d956569-4x88x          1/1     Running   0          75m
prometheus-prometheus-kube-prometheus-prometheus-0     2/2     Running   1          73m
prometheus-prometheus-node-exporter-x2sdt              1/1     Running   0          75m
```
Hello Prometheus. Grafana has also been installed which wil be helpful for visualisation later.
<br><br>

# Helm values.yaml

Custom configuration can be deployed using the **[values.yaml](https://github.com/prometheus-community/helm-charts/blob/main/charts/kube-prometheus-stack/values.yaml)** file.

e.g. if you want to do an install without Grafana? Easy. Just pass this **`values.yaml`** file into the build.
```yaml
## Using default values from https://github.com/grafana/helm-charts/blob/main/charts/grafana/values.yaml
##
grafana:
  ## Deploy Grafana
  ##
  enabled: false
```
```
$ helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring -f values.yaml
```

Redployments can be handled nicely with **`helm upgrade`** too. 
```
$ helm upgrade --install --namespace monitoring prometheus stable/prometheus-operator -f values.yaml
```
<br><br>

# Exposing the web dashboards

To expose the Prometheus or Grafana web dashboards, a couple of solutions could be used. 
* You could create a Kubernetes **`Ingress`** resource, 
* or a Kubernetes **`NodePort`** resource,
* or use the Kubernetes forwarding command **`kubectl port-forward`**.

`NodePort` and `port-forward` can expose a Kubernetes [Service](https://kubernetes.io/docs/concepts/services-networking/service/) resource outside of the cluster via the kube proxy. `Ingress` is a comprehensive load balancing solution, great for a production environment but a bit overkill for now.

`port-forward` is what we're going to use. It allows use to access a service quickly, without permanently exposing it. This is great from a security perspective and for debugging too.

Let's take a look at the Kubernetes [Service](https://kubernetes.io/docs/concepts/services-networking/service/) resources that have been deployed with that install.
```
[minikube@control-plane helpers]$ kubectl get svc -n monitoring
NAME                                    TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
prometheus-grafana                      NodePort    10.96.7.160      <none>        80:30080/TCP   98m
prometheus-kube-prometheus-operator     ClusterIP   10.96.18.46      <none>        443/TCP        98m
prometheus-kube-prometheus-prometheus   ClusterIP   10.100.249.35    <none>        9090/TCP       98m
prometheus-kube-state-metrics           ClusterIP   10.104.133.177   <none>        8080/TCP       98m
prometheus-operated                     ClusterIP   None             <none>        9090/TCP       97m
prometheus-prometheus-node-exporter     ClusterIP   10.103.119.94    <none>        9100/TCP       98m
```
Which one shall we forward? Naming conventions are a little funky but the `9090` ports look promising. Let's try one and forward it to outside of the cluster so we can reach it on our server.
```
[minikube@control-plane kube-system]$ kubectl port-forward svc/prometheus-kube-prometheus-prometheus 9090 -n monitoring
Forwarding from 127.0.0.1:9090 -> 9090
Forwarding from [::1]:9090 -> 9090
```
Good so that sessions started. In another [Shell](https://en.wikipedia.org/wiki/Shell_(computing)) session let's see if we can reach that address outside of the cluster. 
```
minikube@control-plane helpers]$ curl 127.0.0.1:9090
<a href="/graph">Found</a>.
```
Great. If there were any issues we could debug from the trace of the `port-forward` session and look at the logs of the actual pod too.
<br><br>
# Looking at Targets in the Web UI

Ok, so now what? How is discovery configured with Prometheus? Let's head to our Prometheus dashboard and look at **`Targets`**.<br><br>
![](https://user-images.githubusercontent.com/26765027/99829591-66fc7380-2b54-11eb-9532-146e23f39a3b.png)

Focusing on **`Unhealthy`** targets, we're seeing an error which helps to understand Prometheus metric discovery. 
```
Get "http://172.17.0.2:10252/metrics": dial tcp 172.17.0.2:10252: connect: connection refused
```
Prometheus metric discovery is a `Get` - it's a pull request. But it _isn't_ a pull request to specific pods. It is a pull request to their **`Service`** resource. It 'scrapes' services to get metrics. In this case, the service **`prometheus-kube-prometheus-kube-controller-manager`** in the namespace **`monitoring`** doesn't exist. Let's ignore this error for now.
<br><br>
# Exposing metrics for Prometheus
Let's keep this short and sweet. To add services to Prometheus, this is what's required.

* A **[CoreOS ServiceMonitor](https://docs.openshift.com/container-platform/4.4/rest_api/monitoring_apis/servicemonitor-monitoring-coreos-com-v1.html)** resource deployed in _the same_ namespace as the Prometheus pod,
* A **`/metrics`** endpoint on the pod you want to poll,
* A **[Service](https://kubernetes.io/docs/concepts/services-networking/service/)** resource that exposes this metrics endpoint.

Remember, Prometheus scrapes Kubernetes _Services_ **not** Pods. A diagram from [Sysdig.com](https://sysdig.com/blog/kubernetes-monitoring-prometheus-operator-part3/) will make this easier to understand I'm sure and we'll walk through the running order shortly.<br><br> 

![](https://478h5m1yrfsa3bbe262u7muv-wpengine.netdna-ssl.com/wp-content/uploads/2018/09/prometheus_operator_servicemonitor.png)

Not in this diagram is that whole namespace thing I mentioned. A ServiceMonitor resource _has_ to be deployed in the same namespace as the Prometheus pod (`monitoring` in our case) _but_ that ServiceMonitor resource can expose services in _all other namespaces_ to Prometheus. 

I'm not going to deep dive into Prometheus Cluster Resource Discovery (**`CRD`**) but you can read more about it [here](https://coreos.com/operators/prometheus/docs/latest/design.html) and look at the CRD for your estate with `kubectl get crd`.
<br><br>
# Configuring the metrics endpoint on a pod

Taking our Traefik ingress controller as an example. Let's configure the metrics endpoint. The app supports exposing metrics for Prometheus, so it's just a question of passing those args into the build and redploying. This should do the trick. 

```yaml
# traefik-deployment.yaml

...
    spec:
      serviceAccountName: traefik
      terminationGracePeriodSeconds: 60
      containers:
      - image: traefik:v1.7-alpine
        name: traefik
        ports:
        - name: app-services
          containerPort: 80
        - name: dashboard
          containerPort: 8080
        args:
        - "--api"
        - "--kubernetes"
        - "--logLevel=INFO"
        - "--metrics"
        - "--metrics.prometheus.buckets=0.1,0.3,1.2,5.0"
  ```
  
  
Now let's check if the endpoint exists. First let's find the Traefik Service.
```
[minikube@control-plane helpers]$ kubectl get svc -n kube-system
NAME                                                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                        AGE
kube-dns                                             ClusterIP   10.96.0.10      <none>        53/UDP,53/TCP,9153/TCP         51d
metrics-server                                       ClusterIP   10.96.133.225   <none>        443/TCP                        31d
prometheus-kube-prometheus-coredns                   ClusterIP   None            <none>        9153/TCP                       143m
prometheus-kube-prometheus-kube-controller-manager   ClusterIP   None            <none>        10252/TCP                      143m
prometheus-kube-prometheus-kube-etcd                 ClusterIP   None            <none>        2379/TCP                       143m
prometheus-kube-prometheus-kube-proxy                ClusterIP   None            <none>        10249/TCP                      143m
prometheus-kube-prometheus-kube-scheduler            ClusterIP   None            <none>        10251/TCP                      143m
prometheus-kube-prometheus-kubelet                   ClusterIP   None            <none>        10250/TCP,10255/TCP,4194/TCP   141m
prometheus-prometheus-oper-kubelet                   ClusterIP   None            <none>        10250/TCP,10255/TCP,4194/TCP   8d
traefik                                              ClusterIP   10.103.201.54   <none>        80/TCP,8080/TCP                2d3h
traefik-web-ui                                       ClusterIP   10.100.129.98   <none>        80/TCP                         2d
```

Then port-forward.
```
[minikube@control-plane helpers]$ kubectl port-forward svc/traefik 8080 -n kube-system
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```
And a curl adding **`/metrics`** as that should be busy now. 
```
[minikube@control-plane kube-system]$ curl 127.0.0.1:8080/metrics | head
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  7643  100  7643    0     0   # HELP go_gc_duration_seconds A summary of the GC invocation durations.
1# TYPE go_gc_duration_seconds summary
35go_gc_duration_seconds{quantile="0"} 1.0659e-05
kgo_gc_duration_seconds{quantile="0.25"} 3.1354e-05
  go_gc_duration_seconds{quantile="0.5"} 8.6276e-05
  go_gc_duration_seconds{quantile="0.75"} 0.000156634
  go_gc_duration_seconds{quantile="1"} 0.073474265
0go_gc_duration_seconds_sum 0.19274691
 go_gc_duration_seconds_count 258
--# HELP go_goroutines Number of goroutines that currently exist.
100  7643  100  7643    0     0   135k      0 --:--:-- --:--:-- --:--:--  133k
(23) Failed writing body
```

Great.
<br><br>
# Checking current deployment and creating a Service

But what does that Service look like? Will it be configured correctly for the ServiceMonitor resource we're going to create? Let's take a look. For this to work, I'm going to take a top down approach. Let's see how the current ServiceMonitor resources in this deployment should be confgured. 

```
$ kubectl get prometheus -o yaml -n monitoring
```
```
...
    serviceMonitorNamespaceSelector: {}
    serviceMonitorSelector:
      matchLabels:
        release: prometheus
...
```
At the bottom of the trace we see something helpful. We've found the ServiceMonitor configuration for this current deployment. **`NamespaceSelector:{}`** okay, this means ServiceMonitor resources in _all_ namespaces can be selected. But the next block includes another condition, each **`ServiceMonitor`** resource must have the label **`release: prometheus`**. Fine.

Let's create a Service for our Traefik ingress controller. Note from above, we know the path for our metrics endpoint is **`8080/metrics`**.
```yaml
kind: Service
apiVersion: v1
metadata:
  name: traefik
  namespace: kube-system
  labels:
    app: traefik
  annotations:
    prometheus.io/scrape: "true"
    prometheus.io/path: /metrics
    prometheus.io/port: "80"
spec:
  selector:
     app: traefik
  ports:
    - name: app
      port: 80
      targetPort: 80
    - name: dashboard
      port: 8080
      targetPort: 8080
  type: ClusterIP
```

The `prometheus.io/port` should be switched to `8080` I reckon, and if we wanted to we could rename that port to 'metrics' instead of 'dashboard' - but I don't think that actually makes great sense, as there is a Traefik dashboard.
<br><br>
# Creating a ServiceMonitor resource

Here's one I made earlier. We know our Traefik service works, so just have to get this right and we're there.

```yaml
[minikube@control-plane kube-system]$ cat traefik-svc-monitor.yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: traefik
  namespace: monitoring
  labels:
    app: traefik
    release: prometheus
spec:
  endpoints:
  - path: /metrics
    port: dashboard
    interval: 15s
  namespaceSelector:
    matchNames:
    - kube-system
  selector:
    matchLabels:
      app: traefik
```

Note the label, **`release: prometheus`** this was the criteria that had to be met for this Prometheus build. Hope it looks pretty straightforward. Ok, let's apply it. Remember, this has to be applied in the same namespace as the Prometheus pod. 
```
$ kubectl apply -f traefik-svc-monitor.yaml -n monitoring
```
And shall we check it exists? (Yes..)
```
[minikube@control-plane kube-system]$ kubectl get servicemonitor -n monitoring
NAME                                                 AGE
prometheus-kube-prometheus-apiserver                 168m
prometheus-kube-prometheus-coredns                   168m
prometheus-kube-prometheus-grafana                   168m
prometheus-kube-prometheus-kube-controller-manager   168m
prometheus-kube-prometheus-kube-etcd                 168m
prometheus-kube-prometheus-kube-proxy                168m
prometheus-kube-prometheus-kube-scheduler            168m
prometheus-kube-prometheus-kube-state-metrics        168m
prometheus-kube-prometheus-kubelet                   168m
prometheus-kube-prometheus-node-exporter             168m
prometheus-kube-prometheus-operator                  168m
prometheus-kube-prometheus-prometheus                168m
traefik                                              105m
```
Here gets a little tricky if there are problems. There isn't much visibility for errors. With this build, there are no logs for new ServiceMonitor resources or issues in the Prometheus pod. The best solution I've found is simply visiting the Prometheus dashboard and looking at the **`Targets`** section. 

Let's expose the Prometheus dashboard with `port-forward` and see if anything's happened.
```
[minikube@control-plane kube-system]$ kubectl port-forward svc/prometheus-kube-prometheus-prometheus 9090 -n monitoring
Forwarding from 127.0.0.1:9090 -> 9090
Forwarding from [::1]:9090 -> 9090
```

Now let's go to the browser on the server and look at **`Targets`**. Voila. Looks good! <br><br>
![](https://user-images.githubusercontent.com/26765027/99835247-3d474a80-2b5c-11eb-8576-e6d084ddafff.png)

---

Thanks.
