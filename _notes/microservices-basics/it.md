---
title: "Fondamenti dei Microservizi"
categories: [microservices]
tags: [architecture, tools]
lang: it
locale: it
weight: 1
ref: note-microservices-basics
---
Ogni microservizio gestisce una singola funzione di business end-to-end in modo indipendente dagli altri microservizi. Comunicano tra loro tramite protocolli comuni e leggeri come HTTP o message queues. In questo modo è possibile utilizzare linguaggi di programmazione diversi per ciascun microservizio.  
Tuttavia, è importante essere consapevoli che più microservizi ci sono, maggiore è la complessità. Ad esempio, se un’istanza di microservizio va in errore in produzione, ciò può generare una cascata di outage negli altri microservizi, rendendo difficile identificare la root cause e risolverla in tempi rapidi.

##### Strumenti
- **Containerizzazione:** Aiuta a deployare microservizi in ambienti minimalisti e self-contained (docker)  
- **Orchestrazione dei container:** Gestisce il ciclo di vita dei container (kubernetes)  
- **Automazione delle pipeline:** CI/CD (GitLab)  
- **Messaggistica asincrona:** Decoupla ulteriormente i microservizi tramite message brokers e queues (kafka)  
- **Monitoraggio delle prestazioni:** Monitoraggio delle performance dei microservizi (prometheus)  
- **Logging e audit:** Permette di tracciare tutto ciò che avviene nel sistema (datadog)