---
title: "Minikube"
categories: [kubernetes]
tags: [minikube, kubectl, clusters]
lang: en
locale: en
nav_order: 38
ref: note-minikube
---
Minikube is a free tool designed for learning and experimenting with Kubernetes by running a cluster locally on a computer. It is not intended for production use, as it lacks the security, scalability, and networking capabilities provided by cloud environments.

**Common minikube commands**
- `minikube start:` Start a cluster  
- `minikube update-check:` Check for updates  
- `minikube stop:` Stop the cluster  
- `minikube delete:` Delete the cluster  

##### Explore a cluster with kubectl
The minikube command is used to create and manage a local cluster, while kubectl is the tool for interacting with that cluster. In cloud environments, a provider’s CLI typically generates the cluster, and kubectl is then used for subsequent actions.

**Common kubectl commands**
- `kubectl cluster-info:` Displays the IP address of the Kubernetes control plane and the location of core DNS, the container network interface.  
- `kubectl get nodes:` Check nodes in the cluster  
- `kubectl get namespaces:` Lists the namespaces in the cluster. Namespaces isolate and manage applications and services within the cluster.  
- `kubectl get pods -A:` Shows all pods across namespaces. Pods are how containers are run in Kubernetes, including the software required for the cluster itself.  
- `kubectl get services -A:` Lists the services running in the cluster. Services act as internal load balancers, directing traffic to pods.  

<small> Source: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>