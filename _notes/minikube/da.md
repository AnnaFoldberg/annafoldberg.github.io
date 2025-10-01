---
title: "Minikube"
categories: [kubernetes]
tags: [minikube, kubectl, clusters]
lang: da
locale: da
nav_order: 38
ref: note-minikube
---
Minikube er et gratis værktøj udviklet til at lære og eksperimentere med Kubernetes ved at køre et cluster lokalt på en computer. Det er ikke beregnet til produktion, da det mangler den sikkerhed, skalerbarhed og netværksfunktionalitet, som cloud-miljøer tilbyder.

**Almindelige minikube-kommandoer**
- `minikube start:` Starter et cluster  
- `minikube update-check:` Tjekker for opdateringer  
- `minikube stop:` Stopper clusteret  
- `minikube delete:` Sletter clusteret  

##### Udforsk et cluster med kubectl
Minikube-kommandoen bruges til at oprette og administrere et lokalt cluster, mens kubectl er værktøjet til at interagere med det cluster. I cloud-miljøer genererer en udbyders CLI typisk clusteret, hvorefter kubectl bruges til de efterfølgende handlinger.

**Almindelige kubectl-kommandoer**
- `kubectl cluster-info:` Viser IP-adressen på Kubernetes control plane og placeringen af core DNS, container-netværksinterfacet.  
- `kubectl get nodes:` Viser nodes i clusteret  
- `kubectl get namespaces:` Viser namespaces i clusteret. Namespaces isolerer og administrerer applikationer og services inden for clusteret.  
- `kubectl get pods -A:` Viser alle pods på tværs af namespaces. Pods er måden, containers køres på i Kubernetes, inklusive den software der kræves til selve clusteret.  
- `kubectl get services -A:` Viser de services, der kører i clusteret. Services fungerer som interne load balancere, der sender trafik videre til pods.  

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>