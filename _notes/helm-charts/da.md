---
title: "Helm Charts i Kubernetes"
categories: [kubernetes]
tags: [helm, charts, packages, manifests]
lang: da
locale: da
nav_order: 62
ref: note-helm-charts
---

Helm er en package manager for Kubernetes, der gør det nemmere at installere og administrere applikationer i et cluster. Hvor Kubernetes normalt kræver flere manifester (deployments, services osv.), samler Helm disse i én samlet pakke kaldet et Helm Chart.

Et Helm Chart definerer alle de Kubernetes-ressourcer, der hører til en applikation, og gør det muligt at installere, opgradere og afinstallere dem som en samlet enhed.

Et Helm Chart består typisk af:

- Templated YAML-manifester  
- En values-fil til konfiguration  
- En Chart.yaml med metadata  

Ved installation udfylder Helm templaterne med værdier, genererer færdige Kubernetes-manifester og anvender dem på clusteret.

##### Community Charts

Helm tilbyder genbrug af community-maintained charts til populære teknologier. Når man installerer f.eks. Kong eller RabbitMQ via Helm (f.eks. fra https://artifacthub.io), henter man et eksisterende chart og får automatisk alle nødvendige ressourcer oprettet i clusteret.

##### Egne Charts

Man kan også oprette sine egne Helm Charts til interne services. Det kan være nyttigt, hvis applikationen består af mange ressourcer, har kompleks konfiguration, eller skal kunne installeres ensartet på flere miljøer.

For enkle applikationer med få manifester giver egne charts ofte begrænset værdi og kan i stedet introducere unødig kompleksitet.

<small>Kilde: [Helm Documentation](https://helm.sh/)</small>