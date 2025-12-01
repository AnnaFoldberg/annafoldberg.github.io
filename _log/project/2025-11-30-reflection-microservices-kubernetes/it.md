---
title: "2025-11-30: Il Ruolo di Microservizi e Kubernetes nel Progetto"
categories: [microservices, kubernetes, trackunit]
tags: [reflection, project, individual]
lang: it
locale: it
nav_order: 18
ref: log-2025-11-30-reflection-microservices-kubernetes
---

##### Microservizi vs Monolite

L’architettura a microservizi nel progetto ha fornito una chiara suddivisione delle responsabilità, in cui ogni servizio aveva il proprio scopo e poteva essere sviluppato, distribuito e testato in modo indipendente. Questo ha reso più semplice lavorare in parallelo nel team e concentrarsi su funzioni separate senza il rischio che modifiche in un punto bloccassero il lavoro in altre parti del sistema.

Tuttavia, considerando le dimensioni attuali del sistema, un monolite avrebbe potuto essere più semplice e più veloce da sviluppare. Il sistema non sfrutta pienamente i vantaggi della comunicazione asincrona, poiché il flusso è in pratica sequenziale. Un monolite avrebbe quindi ridotto la complessità senza limitare la funzionalità.

I microservizi hanno quindi funzionato principalmente come strumento di apprendimento, permettendomi di lavorare con logiche di servizio, isolamento e scalabilità. Hanno anche supportato la collaborazione del team, perché responsabilità e confini erano chiaramente definiti. D’altra parte, ciò ha richiesto particolare attenzione all’integrazione e ai contratti tra i servizi, affinché il ruolo di ciascun microservizio rimanesse chiaro.

##### Kubernetes vs docker-compose

Nella fase di prototipo del progetto ho utilizzato docker-compose per eseguire l’applicazione e i suoi componenti infrastrutturali. Questo ha fornito un ambiente semplice e rapido, in cui tutti i servizi potevano essere avviati insieme in un’unica rete.

Il passaggio a Kubernetes tramite kind ha introdotto invece numerosi vantaggi operativi, tra cui:

- **Riavvio automatico** dei pod in caso di errore  
- **Rolling updates** quando vengono distribuite nuove versioni delle immagini  
- **Scalabilità** con più repliche nei casi di maggiore carico  
- **Network Policies** per controllare il traffico di rete  
- **Namespaces** per isolare i workloads  
- **Punto d’ingresso controllato** tramite un servizio LoadBalancer, mentre i servizi interni restano inaccessibili dall’esterno tramite ClusterIP  
- **Helm charts** per configurazioni di deployment stabili e ben strutturate dei componenti infrastrutturali

Kubernetes ha fornito al sistema un modello operativo più robusto, scalabile e realistico, con accesso a meccanismi di sicurezza come TLS, Ingress, secrets e network policies, e ha reso possibile dimostrare una reale orchestrazione e sicurezza in un ambiente cloud-native.