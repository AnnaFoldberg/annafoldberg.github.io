---
title: "Oprettelse af Namespaces i Kubernetes"
categories: [kubernetes]
tags: [namespaces, manifests, kubectl]
lang: da
locale: da
nav_order: 40
ref: note-namespaces-kubernetes
---
Namespaces i Kubernetes bruges til at isolere og organisere workloads. De gør det muligt at adskille miljøer eller applikationer inden for det samme cluster, fx ved at bruge ét namespace til udvikling og et andet til produktion.

##### Standard namespaces
Et nyt Kubernetes-cluster leveres med fire namespaces som standard:

- default  
- kube-system  
- kube-public  
- kube-node-lease  

##### Namespace-manifester
Namespaces defineres ved hjælp af YAML-manifester. Flere namespaces kan erklæres i samme fil ved at adskille dokumenter med `---`.

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

##### Administrationskommandoer
- **Opret namespaces fra et manifest:** `kubectl apply -f namespace.yaml`  
- **Vis alle namespaces:** `kubectl get namespaces`  
- **Slet namespaces defineret i et manifest:** `kubectl delete -f namespace.yaml`  

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>