---
title: "Services in Kubernetes"
categories: [kubernetes]
tags: [services, clusterip, nodeport, loadbalancer]
lang: en
locale: en
nav_order: 60
ref: note-kubernetes-services
---

A Service in Kubernetes is a stable access point that allows one or more Pods to be reached through a fixed IP and port. Since Pods frequently restart, move, or scale up and down, a Service ensures that clients do not need to track their changing IP addresses.

A Service groups a set of Pods, typically using a selector, and exposes them as a single logical endpoint. This means that applications inside or outside the cluster can always reach the same address, regardless of which Pods are currently running.

Kubernetes provides three primary service types — ClusterIP, NodePort, and LoadBalancer — which determine whether the service is internal only, reachable via node IPs, or exposed through an external load balancer.

##### ClusterIP (default)  
Exposes the service internally within the cluster through an internal cluster IP. Used for Pod-to-Pod communication and provides no external access. Kubernetes creates this service type if no other type is specified.

##### NodePort  
Makes the service accessible from outside the cluster by opening a static port (NodePort) on every node. The port range is 30000–32767 and can be accessed directly using e.g. http://\<node-ip\>:30080.

Kubernetes also creates an internal ClusterIP in the same way as for a ClusterIP service, so the service works both internally and externally. All traffic sent to nodeIP:nodePort is automatically forwarded to the service and then to the associated Pods.

**Drawbacks of NodePort**  
To reach the service externally, the client must know the IP addresses of the nodes, and it is up to the client to choose which node to contact. If the client targets a node that later goes down, the service will appear unavailable, even though the NodePort remains open on the remaining nodes. For this reason, NodePort is primarily used for development, testing, kind environments, and simple proof-of-concepts where high availability is not critical.

##### LoadBalancer  
Exposes the service externally using an external load balancer. It automatically distributes traffic to the nodes and Pods running the service and is commonly used in production due to its stability and accessibility.

Kubernetes does not provide a load balancer by itself. It must therefore be added manually or provided by a cloud vendor.

In cloud environments, the provider’s load balancer handles routing to the appropriate nodes. In kind, the LoadBalancer type only works if a tool that simulates an external load balancer is installed (e.g. cloud-provider-kind).

<small>Source: [Kubernetes Documentation](https://kubernetes.io/docs/concepts/services-networking/service/)</small>