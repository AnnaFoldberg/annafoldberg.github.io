---
title: "Aggiunta di Resource Requests e Limits"
categories: [kubernetes]
tags: [resources, requests, limits]
lang: it
locale: it
nav_order: 46
ref: note-resource-requests-limits
---
Kubernetes può riservare CPU e memoria per i Pods quando vengono definiti resource requests e limits, garantendo che i Pods vengano pianificati solo su nodes con capacità sufficiente e impedendo alle applicazioni di consumare troppe risorse. Impostare questi requests e limits è un passaggio essenziale per evitare errori dei nodes e interruzioni nei clusters Kubernetes.

##### Perché è importante
- **Senza requests:** I Pods possono essere pianificati su nodes senza risorse sufficienti, causando instabilità o errori.  
- **Senza limits:** Un Pod può consumare tutta la CPU o memoria disponibile su un node, potenzialmente bloccando altri workloads e causando interruzioni.  

##### Manifest Deployment
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

##### Spiegazione del manifest
- **requests:** Risorse minime richieste per pianificare il Pod (64Mi memoria, 250m CPU).  
- **limits:** Risorse massime che il Pod può usare prima di essere interrotto (128Mi memoria, 500m CPU).  
- **Note sulle unità:**  
  - Memory units: Mi = mebibytes.  
  - CPU units: m = millicores (1000m = 1 CPU core).  

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>