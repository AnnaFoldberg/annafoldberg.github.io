---
title: "Kubernetes-arkitektur: Cluster Components"
categories: [kubernetes]
tags: [architecture, cluster, control-plane, worker-nodes]
lang: da
locale: da
nav_order: 51
ref: note-kubernetes-architecture-cluster-components
---
Et Kubernetes-cluster består af control plane-komponenter, der styrer systemet, og worker node-komponenter, der kører applikationerne.

##### Control Plane Components
- **Cloud Controller Manager:** Forbinder et Kubernetes-cluster til en cloud-udbyders API, håndterer cloud-specifikke ressourcer og sikrer korrekt integration med den underliggende infrastruktur  
- **etcd:** En key-value store, der gemmer alle data om clusterets tilstand; kun kube-apiserver kan kommunikere direkte med etcd  
- **kube-apiserver:** Et centralt komponent i Kubernetes, der eksponerer Kubernetes API’et, håndterer de fleste forespørgsler og styrer interaktionen med clusteret ved at behandle og validere API-forespørgsler – afgørende for clusterets funktion  
- **kube-controller-manager:** Overvåger Kubernetes-clusterets tilstand og kører processer for at sikre, at den aktuelle tilstand matcher den ønskede tilstand  
- **kube-scheduler:** Identificerer nyligt oprettede Pods, der ikke er tildelt en worker node, og tildeler dem til en specifik node  

##### Worker Node Components
- **Container Runtime:** Henter container-images, opretter og administrerer containere og sikrer, at de kører korrekt og sikkert som instrueret af Kubernetes control plane  
- **kube-proxy:** En netværks-proxy, der kører på hver node i et Kubernetes-cluster, opretholder netværksregler og muliggør kommunikation mellem Pods og Services på tværs af worker nodes og control plane, samtidig med at den kommunikerer direkte med kube-apiserver  
- **kubelet:** En agent, der kører på hver node i et Kubernetes-cluster, sikrer, at containere i en Pod kører og er healthy, og kommunikerer med API-serveren i control plane for at opretholde nodens ønskede tilstand  

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>