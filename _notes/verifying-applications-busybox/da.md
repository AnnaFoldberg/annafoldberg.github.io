---
title: "Verificering af Applikationer med BusyBox"
categories: [kubernetes]
tags: [busybox, debugging, troubleshooting]
lang: da
locale: da
nav_order: 43
ref: note-verifying-applications-busybox
---
Efter at have deployet en applikation i Kubernetes er det ofte nødvendigt at verificere, at den fungerer korrekt. En måde at gøre dette på er ved at bruge BusyBox, et letvægts Linux-værktøj, der samler mange almindelige Unix-tools (fx awk, date, whoami, wget) i en enkelt binær. Fordi Kubernetes kører på Linux, er BusyBox et nyttigt værktøj til debugging og fejlfinding inde i et cluster.

##### Deployment
BusyBox kan deployes som en Pod ved hjælp af et Deployment-manifest. I modsætning til applikations-deployments køres den typisk kun med én replica i default-namespace.

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
        command: [ "sleep", "3600" ]  # holder containeren kørende
```

##### Vigtige kommandoer
- **Opret Pod:** `kubectl apply -f busybox.yaml`  
- **Tjek Pod:** `kubectl get pods`. Som standard i default-namespace.  
- **Få Pod IPs:** `kubectl get pods -n development -o wide`. Viser IP-adresser for applikations-Pods.  
- **Tilgå BusyBox:** `kubectl exec -it <busybox-pod> -- /bin/sh`. Åbner en interaktiv shell i Pod’en.  

##### Test af applikationen
- Brug `wget <pod-ip-address>:<port>` inde i BusyBox-Pod’en for at teste forbindelsen til applikations-Poddens IP-adresse. Sørg for at angive den korrekte port (fx 3000 i stedet for standardporten 80).  
- Lykkedes anmodningen, gemmes svaret (fx som index.html).  

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>