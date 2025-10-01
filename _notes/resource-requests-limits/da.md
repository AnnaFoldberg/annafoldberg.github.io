---
title: "Tilføjelse af Resource Requests og Limits"
categories: [kubernetes]
tags: [resources, requests, limits]
lang: da
locale: da
nav_order: 46
ref: note-resource-requests-limits
---
Kubernetes kan reservere CPU og memory til Pods, når resource requests og limits defineres. Det sikrer, at Pods kun placeres på nodes med tilstrækkelig kapacitet, samtidig med at det forhindrer applikationer i at bruge for mange ressourcer. At sætte disse requests og limits er et vigtigt skridt for at undgå node-fejl og nedetid i Kubernetes-clusters.

##### Hvorfor det er vigtigt
- **Uden requests:** Pods kan blive placeret på nodes uden nok ressourcer, hvilket kan skabe ustabilitet eller fejl.  
- **Uden limits:** En Pod kan bruge al tilgængelig CPU eller memory på en node, hvilket kan crashe andre workloads og skabe nedetid.  

##### Deployment-manifest
```yaml
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
```

##### Gennemgang af manifest
- **requests:** Minimum ressourcer der kræves for at planlægge Pod’en (64Mi memory, 250m CPU).  
- **limits:** Maksimum ressourcer Pod’en må bruge, før den stoppes (128Mi memory, 500m CPU).  
- **Noter om units:**  
  - Memory units: Mi = mebibytes.  
  - CPU units: m = millicores (1000m = 1 CPU core).  

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>