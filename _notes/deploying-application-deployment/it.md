---
title: "Deployment di un’Applicazione con una Deployment"
categories: [kubernetes]
tags: [deployments, pods, replicas]
lang: it
locale: it
nav_order: 41
ref: note-deploying-application-deployment
---
Kubernetes è progettato per rendere le applicazioni altamente disponibili eseguendo più replicas contemporaneamente. Se una replica fallisce, le altre rimangono attive per gestire il traffico. I Pods sono la risorsa Kubernetes che esegue applicazioni e microservizi. Un Deployment garantisce che i Pods siano organizzati, replicati e mantenuti in esecuzione.

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

##### Spiegazione del manifest
- **apiVersion:** apps/v1, il gruppo API utilizzato per i deployments.  
- **kind:** Deployment, specifica l’oggetto Kubernetes.  
- **metadata:** Include il nome del deployment, il namespace e le labels per i Pods.  
- **spec.replicas:** Numero di Pod replicas da mantenere (impostato su 3 per alta disponibilità).  
- **containers:** Definisce il container da eseguire, la sua image (kimschles/pod-info-app:latest), la porta esposta (3000) e le environment variables (POD_NAME, POD_NAMESPACE, POD_IP).  

##### Comandi di gestione
- **Assicurarsi che il namespace di destinazione esista:** `kubectl apply -f namespace.yaml`  
- **Creare il deployment:** `kubectl apply -f deployment.yaml`  
- **Elencare i deployments nel namespace development:** `kubectl get deployments -n development`  
- **Visualizzare i Pods creati dal deployment:** `kubectl get pods -n development`  
- **Eliminare un Pod e osservare il deployment crearne automaticamente uno nuovo:** `kubectl delete pod <pod-name> -n development`  

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>