---
title: "Verifica delle Applicazioni con BusyBox"
categories: [kubernetes]
tags: [busybox, debugging, troubleshooting]
lang: it
locale: it
nav_order: 43
ref: note-verifying-applications-busybox
---
Dopo aver effettuato il deployment di un’applicazione in Kubernetes, è spesso necessario verificare che funzioni correttamente. Un modo per farlo è utilizzare BusyBox, una utility Linux leggera che raccoglie molti comuni strumenti Unix (ad es. awk, date, whoami, wget) in un unico binario. Poiché Kubernetes gira su Linux, BusyBox è uno strumento utile per debugging e troubleshooting all’interno di un cluster.

##### Deployment
BusyBox può essere deployato come Pod utilizzando un Deployment manifest. A differenza dei deployment di applicazioni, viene in genere eseguito con una sola replica nel namespace default.

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: busybox-deployment
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: busybox
  template:
    metadata:
      labels:
        app: busybox
    spec:
      containers:
      - name: busybox
        image: busybox:latest
        command: [ "sleep", "3600" ]  # mantiene il container in esecuzione
```

##### Comandi principali
- **Creare il Pod:** `kubectl apply -f busybox.yaml`  
- **Verificare il Pod:** `kubectl get pods`. Per default nel namespace default.  
- **Ottenere IP dei Pod:** `kubectl get pods -n development -o wide`. Mostra gli indirizzi IP dei Pods applicativi.  
- **Accedere a BusyBox:** `kubectl exec -it <busybox-pod> -- /bin/sh`. Apre una shell interattiva all’interno del Pod.  

##### Test dell’applicazione
- Usa `wget <pod-ip-address>:<port>` dentro il Pod BusyBox per testare la connettività verso l’indirizzo IP del Pod applicativo. Assicurati di specificare la porta corretta (ad es. 3000 invece della porta predefinita 80).  
- Le richieste andate a buon fine salvano la risposta (ad es. come index.html).  

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>