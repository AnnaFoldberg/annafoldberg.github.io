---
title: "Deleting Kubernetes Objects and Tearing Down a Cluster"
categories: [kubernetes]
tags: [cluster, minikube]
lang: en
locale: en
nav_order: 47
ref: note-deleting-objects-teardown-cluster
---
Because Minikube is resource-intensive, it is best to clean up resources and stop the cluster when it is not in use. This involves deleting Kubernetes objects and then shutting down the Minikube cluster.

##### Deleting the Cluster
- `minikube delete:` Removes the local Minikube cluster and frees system resources.  

<small> Source: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>