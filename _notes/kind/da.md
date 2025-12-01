---
title: "Kind"
categories: [kubernetes]
tags: [kind, clusters]
lang: da
locale: da
nav_order: 55
ref: note-kind
---

Kind er et værktøj, der gør det muligt at køre Kubernetes-clustre lokalt ved at bruge Docker-containere som noder. Det blev oprindeligt designet til at teste Kubernetes, men kan bruges til lokal udvikling, test og proof-of-concepts, hvor man ønsker et ægte Kubernetes-miljø uden en cloud-udbyder.

Kind opretter et komplet cluster inde i Docker, hvor hver node i clusteret svarer til en separat container.

##### Eksterne IP’er i kind

Da kind ikke kører i en cloud, oprettes der ikke automatisk eksterne load balancere. For at bruge Service-typen LoadBalancer i kind skal man tilføje en komponent, der efterligner en cloud-udbyder. Det gør det muligt at teste komponenter som API-gateways, der normalt kræver en ekstern load balancer.

Alternativt kan man bruge Service-typen NodePort til lokal udvikling.

##### Kind kommandoer

- **Opret cluster:** `kind create cluster`  
- **Slet cluster:** `kind delete cluster`  
- **Simuler cloud-udbyder:** `sudo go/bin/cloud-provider-kind`

<small>Kilde: [kind Documentation](https://kind.sigs.k8s.io/)</small>