---
title: "Architettura Kubernetes: Cluster Components"
categories: [kubernetes]
tags: [architecture, cluster, control-plane, worker-nodes]
lang: it
locale: it
nav_order: 51
ref: note-kubernetes-architecture-cluster-components
---
Un cluster Kubernetes è composto da componenti del control plane che gestiscono il sistema e componenti dei worker nodes che eseguono le applicazioni.

##### Control Plane Components
- **Cloud Controller Manager:** Collega un cluster Kubernetes all’API di un cloud provider, gestendo le risorse specifiche del cloud e garantendo la corretta integrazione con l’infrastruttura sottostante  
- **etcd:** Un key-value store che salva tutti i dati sullo stato del cluster; solo il kube-apiserver può comunicare direttamente con etcd  
- **kube-apiserver:** Un componente chiave di Kubernetes che espone le API di Kubernetes, gestisce la maggior parte delle richieste e controlla le interazioni con il cluster elaborando e convalidando le richieste API – essenziale per il funzionamento del cluster  
- **kube-controller-manager:** Monitora lo stato del cluster Kubernetes, eseguendo processi per garantire che lo stato corrente corrisponda allo stato desiderato  
- **kube-scheduler:** Identifica i Pods appena creati che non sono stati assegnati a un worker node e li assegna a un node specifico  

##### Worker Node Components
- **Container Runtime:** Scarica le immagini dei container, crea e gestisce i container e garantisce che vengano eseguiti correttamente e in sicurezza secondo le istruzioni del control plane di Kubernetes  
- **kube-proxy:** Un proxy di rete che gira su ogni node in un cluster Kubernetes, mantenendo le regole di rete e consentendo la comunicazione tra Pods e Services all’interno dei worker nodes e con il control plane, comunicando anche direttamente con il kube-apiserver  
- **kubelet:** Un agente che gira su ogni node in un cluster Kubernetes, garantendo che i container in un Pod siano in esecuzione e healthy, comunicando con l’API server nel control plane per mantenere lo stato desiderato del node  

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>