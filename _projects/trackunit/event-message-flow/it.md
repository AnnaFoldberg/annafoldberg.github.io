---
title: "Trackunit: Event Message Flow Architecture"
categories: [trackunit, microservices]
tags: [architecture, messaging, events, flow, final]
lang: it
locale: it
nav_order: 50
ref: event-message-flow
---

![event-message-flow-architecture.png](../../../assets/images/projects/trackunit/event-message-flow/event-message-flow-architecture.png)

> **Suggerimento:** Apri il diagramma in una nuova scheda per ingrandire e vedere i dettagli.

Questo diagramma mostra il flusso di comunicazione tra il client, i pod nel cluster Kubernetes e i servizi esterni integrati nel sistema.

##### Upload dell’immagine

1. **trackunit-client** invia la mutation `startUpload` a **graph-gateway**.  
2. **graph-gateway** chiama `POST /uploads/start` in **tu-ingestion-service**.  
3. **tu-ingestion-service** restituisce `PutUrl` e `objectKey`, che **graph-gateway** inoltra al client.  
4. **trackunit-client** carica il file direttamente su **MinIO** tramite `PutUrl`.  
5. Una volta completato l’upload, il client invia `confirmUpload`, che **graph-gateway** inoltra a `POST /uploads/confirm`.  
6. **tu-ingestion-service** pubblica l’evento `tu.image.uploaded` sul topic Kafka.

> **Nota:** Il flusso di upload è mantenuto separato per funzionare indipendentemente da **svc-analysis-orchestrator**.

##### Analisi AI dell’immagine

1. **svc-ai-vision-adapter** consuma `tu.image.uploaded` da Kafka.  
2. Recupera l’immagine tramite **tu-media-access-service**.  
3. La analizza utilizzando Google Cloud Vision.  
4. Pubblica `tu.recognition.completed` su Kafka.

##### Orchestrazione dell’analisi

1. **svc-messaging-bridge** consuma `tu.image.uploaded` e lo ripubblica su RabbitMQ exchange `tu.image.uploaded`.  
2. **svc-analysis-orchestrator** lo consuma e invia `AnalysisStarted` all’exchange `analysis.started`.  
3. **graph-gateway** lo consuma e lo inoltra al client tramite subscription `onAnalysisStarted`.  
4. **svc-ai-vision-adapter** completa l’analisi e pubblica `tu.recognition.completed`.  
5. **svc-messaging-bridge** lo ripubblica su exchange `tu.recognition.completed`.  
6. **svc-analysis-orchestrator** lo consuma e pubblica `AnalysisCompleted` su `analysis.completed`.  
7. **graph-gateway** lo inoltra tramite la subscription `onAnalysisCompleted` al client.

> **Nota:** Anche se l’orchestrator dovrebbe essere l’unico entry point del workflow di analisi, la versione attuale permette a **svc-ai-vision-adapter** di consumare direttamente `tu.image.uploaded` da Kafka per semplificare lo sviluppo iniziale.