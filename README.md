# netsil-kube

## Netsil

[Netsil](http://netsil.com/) discovers your application's topology based on its traffic; it analyzes traffic in a non-intrusive way and offers an insight into the exposed services.

## Install

Requisites
- Running kubernetes cluster
- configured kubectl

The installation process follows 4 simple steps

* Create ```netsil``` namespace
```
$ kubectl create -f netsil-ns.yaml 
namespace "netsil" created
```
* Create ```netsil``` replication controller
```
$ kubectl create -f netsil-rc.yaml 
replicationcontroller "netsil" created
```
* Create ```netsil``` service
At this point you might want to modify ```netsil-svc.yaml``` to change the type of the service to ```LoadBalancer``` if you are using a cloud provider plugin that supports load balancers or create an ```Ingress``` if you have an Ingress Controller.

```
$ kubectl create -f netsil-svc.yaml 
(possible warning to open ports)
service "netsil" created
```

* Create ```collector``` DaemonSet. This will create a Netsil collector agent in every node of the cluster.
```
$ kubectl create -f collector.yaml
daemonset "collector" created
```
## Using netsil

Unless you have modified the port settings in the service files above, these are the ports that should be opened to run Netsil:

Incoming UDP ports: 2003
Incoming TCP ports: 30443, 2001, 2003

All your external services will be monitored now using Netsil.

