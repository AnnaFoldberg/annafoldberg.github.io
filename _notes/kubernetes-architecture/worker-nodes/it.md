---
title: "Architettura Kubernetes: Worker Nodes"
categories: [kubernetes]
tags: [architecture, worker-nodes]
lang: it
locale: it
nav_order: 49
ref: note-kubernetes-architecture-worker-nodes
---
![Kubernetes Cluster Architecture (Airport Analogy)](../../../assets/images/notes/kubernetes-architecture/worker-nodes/control-plane-worker-nodes-airport.png)

Se Kubernetes fosse un aeroporto, il control plane sarebbe la torre di controllo, mentre i worker nodes sarebbero i terminal affollati dove gli aerei parcheggiano e i passeggeri salgono a bordo. Per mantenere un’alta disponibilità, la maggior parte dei clusters funziona con almeno tre worker nodes. I worker nodes sono il luogo in cui i Pods vengono assegnati a un node ed eseguiti, e ogni node ha tre componenti principali.

##### Kubelet
Il Kubelet è un agente che gira su ogni worker node. Garantisce che i container nei Pods vengano avviati e rimangano in salute. Il Kubelet comunica direttamente con l’API server nel control plane e controlla continuamente i Pods appena assegnati.

##### Container Runtime
Il container runtime è responsabile dell’avvio dei container una volta che il Kubelet assegna un Pod. Questo avviene tramite il Container Runtime Interface (CRI), che supporta motori come containerd, CRI-O, Kata Containers e AWS Firecracker. A partire da Kubernetes v1.24, Dockershim è stato rimosso, il che significa che Docker non è più un runtime supportato. Tuttavia, le Docker images funzionano ancora in Kubernetes perché le immagini e i runtimes sono concetti separati.

##### Kube-proxy
Il Kube-proxy garantisce il networking all’interno del cluster, consentendo a Pods e Services di comunicare tra i worker nodes e con il control plane. Ogni Kube-proxy comunica direttamente con l’API server.

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>