---
title: "Trackunit Prototype: Architettura di sistema"
categories: [trackunit, microservices]
tags: [architecture, infrastructure, prototype]
lang: it
locale: it
nav_order: 8
ref: system-architecture
---

##### Servizi principali

- **[GraphGateway](https://github.com/team-2-devs/graph-gateway)**  
  Microservizio che funge da facciata API e gateway applicativo per il sistema. Fornisce un’API GraphQL con query, mutation e subscription. Pubblica i comandi `RequestAnalysis` su RabbitMQ e inoltra gli eventi `analysis/started` e `analysis/completed` ai campi di subscription GraphQL `onAnalysisStarted` e `onAnalysisCompleted`.

- **[SvcAnalysisOrchestrator](https://github.com/team-2-devs/svc-analysis-orchestrator)**  
  Microservizio che orchestra l’analisi. Consuma i comandi `RequestAnalysis` da RabbitMQ e pubblica gli exchange di eventi `analysis.started` e `analysis.completed`.

##### Componenti condivisi

- **[Messaging](https://github.com/team-2-devs/messaging)**  
  Pacchetto NuGet che contiene `MessageContracts` condivisi e interfacce e implementazioni riutilizzabili per i publisher RabbitMQ.

##### Infrastruttura

- **[InfraCore](https://github.com/team-2-devs/infra-core)**  
  Ambiente di orchestrazione basato su Docker Compose che raggruppa tutti i servizi e i componenti infrastrutturali, inclusi **Kong Gateway**, **oauth2-proxy** e **RabbitMQ**.

- **Kong Gateway**  
  API Gateway in modalità *db-less*. Instrada le richieste `/graphql` verso **oauth2-proxy**, che convalida i token in ingresso prima di inoltrarli a **GraphGateway**.

- **oauth2-proxy**  
  Proxy di autenticazione che convalida i token Bearer (JWT) emessi da Microsoft Entra ID.  
  Solo le richieste con token validi vengono inoltrate a **GraphGateway**.

- **RabbitMQ**  
  Message broker per la comunicazione asincrona.