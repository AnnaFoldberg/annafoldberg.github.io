---
title: "Trackunit: Flusso di analisi"
categories: [trackunit, microservices]
tags: [rabbitmq, graphql, architecture]
lang: it
locale: it
nav_order: 15
ref: analysis-flow
---

1. **ClientConsole** apre una connessione WebSocket a `/graphql` tramite Kong Gateway → oauth2-proxy → GraphGateway.  
2. **ClientConsole** avvia una subscription filtrata per `correlationId` oppure si prepara a risottoscriversi quando ne ha uno.  
3. **ClientConsole** chiama la mutation GraphQL `requestAnalysis`, passando un `objectKey`.  
4. **GraphGateway** riceve la mutation, genera un `CorrelationId` e lo restituisce come `AnalysisRequestPayload(correlationId)` nel payload della risposta.  
5. **GraphGateway** pubblica un comando `RequestAnalysis` su RabbitMQ (exchange: `analysis.commands`, routing key: `analysis.request`).  
6. **SvcAnalysisOrchestrator** consuma il comando `RequestAnalysis` dalla propria coda (`orchestrator.analysis.commands`) e simula l’elaborazione.  
7. Durante l’elaborazione, l’orchestratore pubblica **eventi**:  
   - `AnalysisStarted` sull’exchange **analysis.started** (fanout).  
   - `AnalysisCompleted` sull’exchange **analysis.completed** (fanout).  
8. **GraphGateway** ascolta questi exchange e li inoltra come subscription GraphQL:  
   - `AnalysisStarted` → attiva la subscription `onAnalysisStarted`.  
   - `AnalysisCompleted` → attiva la subscription `onAnalysisCompleted`.  
9. **ClientConsole** riceve gli eventi di subscription tramite la connessione WebSocket esistente e filtra per `correlationId` nel payload per correlare la risposta alla propria richiesta.