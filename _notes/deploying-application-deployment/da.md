---
title: "Deployment af en Applikation med en Deployment"
categories: [kubernetes]
tags: [deployments, pods, replicas]
lang: da
locale: da
nav_order: 41
ref: note-deploying-application-deployment
---
Kubernetes er designet til at gøre applikationer højtilgængelige ved at køre flere replicas samtidigt. Hvis en replica fejler, forbliver andre til at håndtere trafikken. Pods er den Kubernetes-ressource, der kører applikationer og microservices. En Deployment sikrer, at Pods er organiseret, replikeret og holdes kørende.

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
      containers:
      - name: pod-info-container
        image: kimschles/pod-info-app:latest
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
- **apiVersion:** apps/v1, API-gruppen brugt til deployments.  
- **kind:** Deployment, specificerer Kubernetes-objektet.  
- **metadata:** Indeholder deployment-navn, namespace og labels for Pods.  
- **spec.replicas:** Antal Pod replicas, der skal vedligeholdes (sat til 3 for høj tilgængelighed).  
- **containers:** Definerer containeren, dens image (kimschles/pod-info-app:latest), eksponeret port (3000) og environment variables (POD_NAME, POD_NAMESPACE, POD_IP).  

##### Administrationskommandoer
- **Sørg for at namespace findes:** `kubectl apply -f namespace.yaml`  
- **Opret deployment:** `kubectl apply -f deployment.yaml`  
- **List deployments i development-namespace:** `kubectl get deployments -n development`  
- **Vis Pods oprettet af deployment:** `kubectl get pods -n development`  
- **Slet en Pod og se deployment automatisk oprette en erstatning:** `kubectl delete pod <pod-name> -n development`  

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>