---
title: "Minikube"
categories: [kubernetes]
tags: [minikube, kubectl, clusters]
lang: it
locale: it
nav_order: 38
ref: note-minikube
---
Minikube è uno strumento gratuito progettato per imparare e fare esperimenti con Kubernetes eseguendo un cluster in locale su un computer. Non è destinato all’uso in produzione, poiché manca delle funzionalità di sicurezza, scalabilità e rete fornite dagli ambienti cloud.

**Comandi minikube comuni**
- `minikube start:` Avvia un cluster  
- `minikube update-check:` Controlla la presenza di aggiornamenti  
- `minikube stop:` Arresta il cluster  
- `minikube delete:` Elimina il cluster  

##### Esplorare un cluster con kubectl
Il comando minikube viene utilizzato per creare e gestire un cluster locale, mentre kubectl è lo strumento per interagire con quel cluster. Negli ambienti cloud, la CLI del provider genera tipicamente il cluster e kubectl viene poi utilizzato per le azioni successive.

**Comandi kubectl comuni**
- `kubectl cluster-info:` Mostra l’indirizzo IP del control plane di Kubernetes e la posizione del core DNS, l’interfaccia di rete dei container.  
- `kubectl get nodes:` Mostra i nodi nel cluster  
- `kubectl get namespaces:` Elenca i namespaces nel cluster. I namespaces isolano e gestiscono applicazioni e servizi all’interno del cluster.  
- `kubectl get pods -A:` Mostra tutti i pods nei vari namespaces. I pods sono il modo in cui i containers vengono eseguiti in Kubernetes, compreso il software necessario per il cluster stesso.  
- `kubectl get services -A:` Elenca i servizi in esecuzione nel cluster. I services agiscono come load balancer interni, indirizzando il traffico ai pods.  

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>