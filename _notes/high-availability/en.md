---
title: "High Availability in Kubernetes"
categories: [kubernetes]
tags: [high-availability, pods, nodes, scaling]
lang: en
locale: en
nav_order: 66
ref: note-high-availability
---

Kubernetes is a container orchestrator that manages the deployment and operation of many containers across a cluster.

##### Pods

When Kubernetes runs an application, it does so by creating pods.

A pod is a running instance of an application. Multiple pods of the same application can exist to ensure high availability.

##### Nodes

Pods run on nodes. A node is a machine in the cluster, such as a virtual server. A cluster consists of multiple nodes. A single node can:

- Run pods from many different applications  
- Run multiple pods of the same application  

Kubernetes decides the placement dynamically.

##### High availability

Kubernetes achieves application high availability by distributing pods across several nodes.

This means:

- If a single pod fails → Kubernetes restarts it.  
- If an entire node fails → all pods on that node disappear.  
- Pods on the remaining nodes continue running.  
- Kubernetes automatically starts new pods on nodes that are still healthy.  

This allows the application to continue running even if an entire machine in the cluster goes down.

##### Scaling and operations

Kubernetes automatically distributes traffic across pods and can quickly start more pods when load increases.  
You can also add or remove worker nodes without interrupting the application.  
Everything is controlled through declarative configuration files (manifests), making operations predictable and automated.

<small>Source: [Udemy: Learn Kubernetes](https://www.udemy.com/course/learn-kubernetes/)</small>