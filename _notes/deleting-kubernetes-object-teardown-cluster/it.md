---
title: "Eliminazione di Kubernetes Objects e Arresto di un Cluster"
categories: [kubernetes]
tags: [deleting, cluster, minikube]
lang: it
locale: it
nav_order: 47
ref: note-deleting-kubernetes-objects-teardown-cluster
---
Poiché Minikube è intensivo in termini di risorse, è meglio ripulire le risorse e arrestare il cluster quando non è in uso. Questo comporta l’eliminazione dei Kubernetes objects e la successiva chiusura del cluster Minikube.

##### Eliminazione del cluster
- `minikube delete:` Rimuove il cluster Minikube locale e libera le risorse di sistema.  

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>