---
title: "Trackunit: Architettura di sistema"
categories: [trackunit, microservices]
tags: [architecture, infrastructure, final]
lang: it
locale: it
nav_order: 19
ref: system-architecture-final
---

>**Nota:** Solo i componenti elencati sotto **InfraCore** sono distribuiti in Kubernetes.

##### Applicazioni client

- **[trackunit-client](https://github.com/Team-2-Devs/trackunit-client)**  
  **Owner:** Eske.  
  Applicazione client ufficiale del progetto Trackunit, sviluppata in Flutter per dispositivi mobili. Gestisce l’autenticazione tramite Microsoft Entra ID e comunica con **graph-gateway** tramite GraphQL. Supporta le mutation `startUpload` e `confirmUpload` e le subscription `onAnalysisStarted` e `onAnalysisCompleted`. Utilizzata per avviare ed eseguire caricamenti di immagini su **MinIO** e ricevere aggiornamenti di analisi in tempo reale.  
  Non supporta TLS nell’ambiente di sviluppo locale a causa delle limitazioni di rete dell’emulatore Android, che impediscono al client mobile di risolvere il nome DNS locale `graphql.local` e di considerare attendibile il certificato autofirmato senza configurazioni aggiuntive.

- **[client-console](https://github.com/Team-2-Devs/client-console)**  
  Applicazione console pensata per gli sviluppatori e utilizzata durante lo sviluppo. Implementa lo stesso flusso di autenticazione e supporta le stesse mutation e subscription GraphQL di **trackunit-client**.  
  Mantenuta nel progetto come client di riferimento per validare TLS tra client e Kong.  

##### Servizi core

- **[graph-gateway](https://github.com/team-2-devs/graph-gateway)**  
  Microservizio che funge da facciata API e gateway applicativo del sistema. Fornisce un’API GraphQL con query, mutation e subscription. Chiama le API REST `POST /uploads/start` e `POST /uploads/confirm` su **tu-ingestion-service**. Consuma gli eventi `analysis.started` e `analysis.completed` da RabbitMQ e inoltra i corrispondenti eventi `analysis/started` e `analysis/completed` ai campi di subscription GraphQL onAnalysisStarted e onAnalysisCompleted.  

- **[svc-analysis-orchestrator](https://github.com/team-2-devs/svc-analysis-orchestrator)**  
  Microservizio che orchestra il processo di analisi. Consuma gli eventi `tu.image.uploaded` e `tu.recognition.completed` da RabbitMQ e pubblica gli eventi `analysis.started` e `analysis.completed` su RabbitMQ.
  >**Nota:** Sebbene **svc-analysis-orchestrator** dovrebbe essere il punto di ingresso centrale per tutti i flussi di analisi, l'implementazione attuale consente temporaneamente a **svc-ai-vision-adapter** di consumare l'evento `tu.image.uploaded` direttamente da **tu-ingestion-service**. Questa scelta è stata presa dal team per semplificare le prime fasi di sviluppo. In una futura iterazione, l’architettura dovrebbe riallinearsi ai confini di responsabilità previsti, con **svc-analysis-orchestrator** come unico consumer dell’evento `tu.image.uploaded`, orchestrando l’intera pipeline di analisi.  

- **[svc-messaging-bridge](https://github.com/team-2-devs/svc-messaging-bridge)**  
  Microservizio che collega gli eventi da Kafka a RabbitMQ. Consuma eventi dai topic Kafka e li ripubblica (`tu.image.uploaded` e `tu.recognition.completed`) su RabbitMQ. Questo consente a ciascun microservizio di interagire solo con il proprio message broker, senza dover gestire integrazione o compatibilità tra broker diversi.

- **[svc-ai-vision-adapter](https://github.com/Team-2-Devs/svc-ai-vision-adapter)**  
  **Owner:** Marlen. Descrizione dalla documentazione del repository.  
  Microservizio che riceve un’immagine tramite un URL presigned, la elabora tramite il provider AI, aggrega i risultati e restituisce una risposta sintetizzata all’orchestratore.  

- **[tu-ingestion-service](https://github.com/Team-2-Devs/tu-ingestion-service)**  
  **Owner:** Kenneth. Descrizione dalla documentazione del repository.  
  Microservizio che gestisce le sessioni di caricamento dei media, valida i metadati e richiede URL presigned PUT allo Storage per i caricamenti dei client.

- **[tu-media-access-service](https://github.com/Team-2-Devs/tu-media-access-service)**  
  **Owner:** Kenneth. Descrizione dalla documentazione del repository.  
  Microservizio che autorizza l’accesso interno ai media e recupera URL presigned GET dallo Storage per i consumer a valle.  

- **[tu-storage-service](https://github.com/Team-2-Devs/tu-storage-service)**  
  **Owner:** Kenneth. Descrizione dalla documentazione del repository.  
  Microservizio che interagisce con l’object storage per emettere URL presigned PUT/GET e applicare politiche di caricamento e download.  

##### Componenti condivisi

- **[Messaging](https://github.com/team-2-devs/messaging)**  
  Pacchetto NuGet contenente `MessageContracts` condivisi e interfacce e implementazioni riutilizzabili per RabbitMQ e Kafka.

##### Infrastruttura

- **InfraCore**  
  Ambiente di orchestrazione basato su Kubernetes che include microservizi e componenti infrastrutturali come **graph-gateway**, **svc-analysis-orchestrator**, **Kong Gateway**, **oauth2-proxy** e **RabbitMQ**.

- **kong-kong**  
  API Gateway in modalità db-less. Instrada le richieste `/graphql` verso **oauth2-proxy**, che valida i token in ingresso prima di inoltrarli a **graph-gateway**.

- **oauth2-proxy**  
  Proxy di autenticazione che valida i Bearer token (JWT) emessi da Microsoft Entra ID tramite OIDC.  
  Solo le richieste con token validi vengono inoltrate a **graph-gateway**.

- **rabbitmq**  
  Message broker per la comunicazione asincrona.