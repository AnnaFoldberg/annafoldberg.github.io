---
title: "2025-11-04: Conclusioni dal Meeting di Progetto"
categories: [trackunit]
tags: [project, group, feed-forward]
lang: it
locale: it
nav_order: 13
ref: log-2025-11-04-project-meeting-conclusions
---

Durante la riunione abbiamo discusso come collegare i nostri servizi.  
La seguente panoramica descrive come mi aspetto che il flusso dei dati e l’interazione tra i componenti del sistema si presentino nella soluzione finale.  

##### Message Broker Bridge
Alcuni servizi utilizzano RabbitMQ, mentre altri usano Kafka puro o Kafka tramite Redpanda.  
Per questo motivo implementerò un piccolo servizio, **svc-messaging-bridge**, che funge da bridge bidirezionale tra Kafka e RabbitMQ, in modo che i consumer di ciascun servizio rimangano invariati.  

##### Caricamento dell’immagine
1. Il **client** richiede al **server GraphQL** di caricare un’immagine.  
2. Il **server GraphQL** genera un `correlationId` univoco e richiama un endpoint REST API in **tu-ingestion-service**.  
3. **tu-ingestion-service** pubblica un evento su un topic Kafka tramite Redpanda.  
4. **tu-storage-service** si iscrive a questo topic, consuma l’evento e crea un nuovo record nel proprio database associato.  
5. **tu-ingestion-service** restituisce un `PutUrl` presigned e un `objectKey` al **server GraphQL**, che li inoltra al **client**.  
6. Il **client** carica il file direttamente nello storage esterno utilizzando il `PutUrl` fornito.  
7. Una volta completato il caricamento, il **client** invia una conferma a **tu-ingestion-service** tramite il **server GraphQL**, che inoltra la richiesta all’endpoint REST del servizio.  
8. **tu-ingestion-service** pubblica un nuovo evento su un topic Kafka tramite Redpanda.  
9. **tu-storage-service** si iscrive a questo topic, consuma l’evento e aggiorna lo stato nel database in *uploaded*.  
10. **tu-ingestion-service** pubblica l’evento `ImageUploaded` su un topic Kafka tramite Redpanda, includendo l’`objectKey` nel payload.  

> Nota: Questo flusso è mantenuto separato da **svc-analysis-orchestrator** per garantire che il processo di upload possa funzionare in modo indipendente. In questo modo opera come un flusso autonomo, non influenzato da eventuali interruzioni di **svc-analysis-orchestrator**.

##### Orchestrazione dell’analisi
1. **svc-messaging-bridge** consuma l’evento `ImageUploaded` dal topic Kafka tramite Redpanda e lo pubblica su un exchange RabbitMQ di tipo fanout.  
2. **svc-analysis-orchestrator** consuma l’evento dalla propria coda RabbitMQ.  
3. **svc-analysis-orchestrator** pubblica un evento `AnalysisStarted` sull’exchange `analysis.started` in RabbitMQ.  
4. **graph-gateway** consuma l’evento `analysis.started` dalla propria coda RabbitMQ e lo inoltra alla subscription GraphQL `onAnalysisStarted` al **client** tramite WebSocket.  
5. **svc-messaging-bridge** consuma l’evento `analysis.started` dalla propria coda RabbitMQ e lo ripubblica su un topic Kafka.  
6. **svc-ai-vision-adapter** si iscrive a questo topic, consuma l’evento e avvia il flusso AI (vedi *Analisi dell’immagine AI*). Al termine del flusso, pubblica un evento su un topic Kafka.  
7. **svc-messaging-bridge** consuma l’evento dal topic Kafka e lo pubblica su un exchange RabbitMQ.  
8. **svc-analysis-orchestrator** consuma l’evento dalla propria coda RabbitMQ.  
9. **svc-analysis-orchestrator** pubblica un evento `AnalysisCompleted` sull’exchange `analysis.completed` in RabbitMQ.  
10. **graph-gateway** consuma l’evento `analysis.completed` dalla propria coda RabbitMQ e lo inoltra alla subscription GraphQL `onAnalysisCompleted` al **client** tramite WebSocket.  

> Nota: È possibile implementare eventi aggiuntivi, come `AnalysisFailed`, se il risultato del flusso AI non è valido.

##### Analisi dell’immagine AI
1. **svc-ai-vision-adapter** si iscrive a un topic Kafka e consuma l’evento.  
2. **svc-ai-vision-adapter** pubblica un evento su un topic Kafka.  
3. **tu-media-access-service** si iscrive a questo topic, consuma l’evento e *esegue operazioni che consentono a **svc-ai-vision-adapter** di accedere all’immagine da analizzare.*  
6. **svc-ai-vision-adapter** analizza l’immagine utilizzando il servizio Google Cloud Vision AI.  
7. **svc-ai-vision-adapter** pubblica un evento su un topic Kafka che include i risultati dell’analisi nel payload.  

##### Prossimi passi
Verrà stabilita una connessione diretta temporanea tra **tu-ingestion-service** e **svc-ai-vision-adapter** per verificare che il meccanismo di pubblicazione in Kafka funzioni correttamente tra i servizi.  
Una volta verificato, **svc-analysis-orchestrator** verrà inserito come servizio intermedio.  
Il sistema sarà presentato durante il prossimo product briefing.