---
title: "Kubernetes Architecture: Cluster Components"
categories: [kubernetes]
tags: [architecture, cluster, control-plane, worker-nodes]
lang: en
locale: en
nav_order: 51
ref: note-kubernetes-architecture-cluster-components
---
A Kubernetes cluster is made up of control plane components that manage the system and worker node components that run applications.

##### Control Plane Components
- **Cloud Controller Manager:** Connects a Kubernetes cluster to a cloud provider's API, managing cloud-specific resources and ensuring proper integration with the underlying infrastructure  
- **etcd:** A key-value store that saves all data about the state of the cluster; only the kube-apiserver can communicate directly with etcd  
- **kube-apiserver:** A key component of Kubernetes that exposes the Kubernetes API, handles most requests, and manages interactions with the cluster by processing and validating API requests, making it essential for the cluster's operation  
- **kube-controller-manager:** Monitors the Kubernetes cluster's state, running processes to ensure the current state matches the desired state  
- **kube-scheduler:** Identifies a newly created Pod that has not been assigned a worker node and assigns it to a specific node  

##### Worker Node Components
- **Container Runtime:** Pulls container images, creates and manages containers, and ensures they run properly and securely as directed by the Kubernetes control plane  
- **kube-proxy:** A network proxy that runs on each node in a Kubernetes cluster, maintaining network rules and enabling communication between Pods and Services within the node and the control plane, while also communicating directly with the kube-apiserver  
- **kubelet:** An agent that runs on each node in a Kubernetes cluster, ensuring containers in a Pod are running and healthy while communicating with the API server in the control plane to maintain the desired state of the node  

<small> Source: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>