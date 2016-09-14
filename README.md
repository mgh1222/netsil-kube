# netsil-kube

## Netsil

[Netsil](http://netsil.com/) discovers your application's topology based on its traffic; it analyzes traffic in a non-intrusive way and offers an insight into the exposed services.

## Cluster Requisites

You will want to create a cluster with sufficient resources to run Kubernetes along with Netsil. We recommend ensuring your master, instance size match up with the following minimum specs: 

- 8 CPU
- 32 GB Memory

This matches a m3.2XLarge at AWS.

Note, only your master needs to be running this spec. Your nodes can run at a smaller spec. 

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

* Create ```netsil-lite``` replication controller
```
$ kubectl create -f netsil-lite-rc.yaml 
replicationcontroller "netsil-lite" created
```
* Create ```netsil-lite``` service
At this point you might want to modify ```netsil-lite-svc.yaml``` to change the type of service to ```LoadBalancer``` if you are using a cloud provider plugin that supports load balancers or create an ```Ingress``` if you have an Ingress Controller.

```
$ kubectl create -f netsil-lite-svc.yaml 
(possible warning to open ports)
service "netsil-lite" created

```

* Create ```collector``` DaemonSet. This will create a Netsil collector agent in every node of the cluster.
```
$ kubectl create -f collector.yaml
daemonset "collector" created
```

## Host Network and Flannel

If you are using Flannel as your network overlay it is possible you might run into the following issue: 

https://github.com/kubernetes/kubernetes/issues/20391

To work around this problem, you will want to run the following command on the host machines: 


iptables -t nat -I POSTROUTING -o flannel.1 -s *host-private-ip* -j MASQUERADE


## Using netsil

Unless you have modified the port settings in the service files above, these are the ports that should be opened to run Netsil:

Incoming UDP ports: 2003
Incoming TCP ports: 30443, 2001, 2003

All your external services will be monitored now using Netsil.

You can access Netsil by hitting your master public IP and appending *30443*. 
