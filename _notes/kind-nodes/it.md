---
title: "Nodi in kind"
categories: [kubernetes]
tags: [kind, clusters, worker-nodes, control-plane]
lang: it
locale: it
nav_order: 56
ref: note-kind-nodes
---

Quando si crea un cluster con `kind create cluster` senza un file di configurazione, Kind crea:

- 1 nodo con il ruolo di control-plane  
- 0 nodi worker separati  

Il nodo control-plane funziona sia come nodo di gestione sia come worker, poiché nei cluster Kubernetes i nodi control-plane possono eseguire pod, e Kind lo consente di default. Il cluster è quindi composto da un singolo nodo che svolge entrambi i ruoli.

In un cluster di produzione la configurazione è diversa:

- I nodi control-plane sono contrassegnati con il taint `node-role.kubernetes.io/control-plane:NoSchedule` per evitare che i workloads vengano schedulati su di essi  
- I nodi worker separati eseguono tutti i pod applicativi  

<small>Fonte: [kind Documentation](https://kind.sigs.k8s.io/docs/user/configuration/#nodes)</small>