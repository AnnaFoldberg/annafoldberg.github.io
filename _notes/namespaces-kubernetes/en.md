---
title: "Creating Namespaces in Kubernetes"
categories: [kubernetes]
tags: [namespaces, manifests, kubectl]
lang: en
locale: en
nav_order: 40
ref: note-namespaces-kubernetes
---
Namespaces in Kubernetes are used to isolate and organize workloads. They allow environments or applications to be separated within the same cluster, such as using one namespace for development and another for production.

##### Default Namespaces
A new Kubernetes cluster comes with four namespaces by default:

- default  
- kube-system  
- kube-public  
- kube-node-lease  

##### Namespace Manifests
Namespaces are defined using YAML manifests. Multiple namespaces can be declared in a single file by separating documents with `---`.

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: development
---
apiVersion: v1
kind: Namespace
metadata:
  name: production
```

##### Management Commands
- **Create namespaces from a manifest:** `kubectl apply -f namespace.yaml`  
- **View all namespaces:** `kubectl get namespaces`  
- **Delete namespaces defined in a manifest:** `kubectl delete -f namespace.yaml`  

<small> Source: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>