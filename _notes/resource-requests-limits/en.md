---
title: "Adding Resource Requests and Limits"
categories: [kubernetes]
tags: [resources, requests, limits]
lang: en
locale: en
nav_order: 46
ref: note-resource-requests-limits
---
Kubernetes can reserve CPU and memory for Pods when resource requests and limits are defined, ensuring that Pods are scheduled only on nodes with enough capacity while also preventing applications from consuming excessive resources. Setting these requests and limits is an essential step in avoiding node failures and outages in Kubernetes clusters.

##### Why It Matters
- **Without requests:** Pods may be scheduled onto nodes without enough resources, causing node instability or failure.  
- **Without limits:** A Pod can consume all available CPU or memory on a node, potentially crashing other workloads and causing outages.  

##### Deployment Manifest
~~~yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pod-info-deployment
  namespace: development
  labels:
    app: pod-info
spec:
  replicas: 3
  selector:
    matchLabels:
      app: pod-info
  template:
    metadata:
      labels:
        app: pod-info
    spec:
      containers: # containers map
      - name: pod-info-container
        image: kimschles/pod-info-app:latest
        resources:
          requests: # do not schedule unless node has at least 64Mi memory and 250m CPU
            memory: "64Mi"
            cpu: "250m"
          limits:  # stop container if it exceeds 128Mi memory and 500m CPU
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 3000
        env:
          - name: POD_NAME
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: POD_NAMESPACE
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
          - name: POD_IP
            valueFrom:
              fieldRef:
                fieldPath: status.podIP
~~~

##### Manifest Breakdown
- **requests:** Minimum resources required to schedule the Pod (64Mi memory, 250m CPU).  
- **limits:** Maximum resources the Pod can use before being stopped (128Mi memory, 500m CPU).  
- **Unit notes:**  
  - Memory units: Mi = mebibytes.  
  - CPU units: m = millicores (1000m = 1 CPU core).  

<small> Source: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>