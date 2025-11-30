---
title: "Helm Charts in Kubernetes"
categories: [kubernetes]
tags: [helm, charts, packages, manifests]
lang: it
locale: it
nav_order: 62
ref: note-helm-charts
---

Helm è un package manager per Kubernetes che semplifica l’installazione e la gestione delle applicazioni in un cluster. Mentre Kubernetes richiede normalmente più manifest (deployments, services, ecc.), Helm li riunisce in un unico pacchetto chiamato Helm Chart.

Un Helm Chart definisce tutte le risorse Kubernetes associate a un’applicazione e permette di installarle, aggiornarle o rimuoverle come un’unica unità.

Un Helm Chart è tipicamente composto da:

- Manifest YAML templati  
- Un file values per la configurazione  
- Un Chart.yaml con i metadati  

Durante l’installazione, Helm compila i template con i valori, genera i manifest Kubernetes completi e li applica al cluster.

##### Community Charts

Helm supporta il riutilizzo di charts mantenuti dalla community per tecnologie popolari. Installando ad esempio Kong o RabbitMQ tramite Helm (ad esempio da https://artifacthub.io), si scarica un chart esistente e tutte le risorse necessarie vengono create automaticamente nel cluster.

##### Charts personalizzati

È possibile creare anche Helm Charts personalizzati per servizi interni. Questo è utile quando l’applicazione contiene molte risorse, ha configurazioni complesse o deve essere distribuita in modo coerente su più ambienti.

Per applicazioni semplici con pochi manifest, creare charts personalizzati aggiunge spesso poca utilità e può introdurre complessità superflua.

<small>Fonte: [Helm Documentation](https://helm.sh/)</small>