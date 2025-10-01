---
title: "Sletning af Kubernetes Objects og Nedlukning af et Cluster"
categories: [kubernetes]
tags: [deleting, cluster, minikube]
lang: da
locale: da
nav_order: 47
ref: note-deleting-kubernetes-objects-teardown-cluster
---
Da Minikube er ressourcekrævende, er det bedst at rydde op i ressourcer og stoppe clusteret, når det ikke er i brug. Dette indebærer at slette Kubernetes objects og derefter lukke Minikube-clusteret ned.

##### Sletning af clusteret
- `minikube delete:` Fjerner det lokale Minikube-cluster og frigør systemressourcer.  

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>