---
title: "Kubernetes-arkitektur: Control Plane og Worker Nodes Interaktion"
categories: [kubernetes]
tags: [architecture, control-plane, worker-nodes]
lang: da
locale: da
nav_order: 50
ref: note-control-plane-worker-nodes-interaction
---
![Kubernetes Control Plane and Worker Nodes (Sequence Diagram)](../../../assets/images/notes/kubernetes-architecture/control-plane-worker-nodes-interaction/control-plane-worker-nodes-sequence.png)

Et sekvensdiagram viser rækkefølgen af handlinger i Kubernetes, når en Pod tildeles en node. Selvom det er en forenklet visning, fremhæver det de vigtigste trin i, hvordan control plane og worker nodes interagerer.

##### Procesoversigt
1. En bruger kører `kubectl apply -f deployment.yaml`.  
2. API-serveren gemmer den nye deployment-tilstand i etcd.  
3. Controller manager tjekker API-serveren for ændringer.  
4. Scheduleren leder efter nyligt oprettede Pods uden node-tildeling.  
5. API-serveren informerer scheduleren om en ventende Pod.  
6. Scheduleren tildeler Pod’en til en node og rapporterer tilbage til API-serveren.  
7. API-serveren opdaterer etcd med den nye tilstand.  
8. Kubelet på den valgte worker node tjekker API-serveren for nyligt tildelte Pods.  
9. API-serveren leverer Pod-specifikationen og binder Pod’en til noden.  
10. Kubelet henter container-image og starter containeren via container runtime.  
11. Kubelet opdaterer Pod-status (healthy eller unhealthy) hos API-serveren.  
12. API-serveren gemmer Pod-status i etcd.  

##### Opsummering
Denne proces viser, hvordan Kubernetes-komponenter arbejder sammen for at sikre, at Pods tildeles en node, oprettes og overvåges. API-serveren fungerer som det centrale knudepunkt og håndterer konstant hundredvis eller tusindvis af forespørgsler for at holde clusteret kørende.

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>