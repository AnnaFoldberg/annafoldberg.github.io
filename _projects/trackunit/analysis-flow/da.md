---
title: "Trackunit Prototype: Analyseflow"
categories: [trackunit, microservices]
tags: [rabbitmq, graphql, architecture, prototype]
lang: da
locale: da
nav_order: 15
ref: analysis-flow
---

1. **ClientConsole** åbner en WebSocket-forbindelse til `/graphql` via Kong Gateway → oauth2-proxy → GraphGateway.  
2. **ClientConsole** starter et subscription filtreret efter `correlationId` eller forbereder sig på at resubscribe, når den har modtaget et.  
3. **ClientConsole** kalder GraphQL-mutation `requestAnalysis` og sender en `objectKey`.  
4. **GraphGateway** modtager mutationen, genererer et `CorrelationId` og returnerer det som `AnalysisRequestPayload(correlationId)` i mutationssvaret.  
5. **GraphGateway** publicerer en `RequestAnalysis`-command til RabbitMQ (exchange: `analysis.commands`, routing key: `analysis.request`).  
6. **SvcAnalysisOrchestrator** consumer kommandoen `RequestAnalysis` fra sin queue (`orchestrator.analysis.commands`) og simulerer behandlingen.  
7. Efterhånden som arbejdet skrider frem, publicerer orchestratoren events:  
   - `AnalysisStarted` til **analysis.started** fanout-exchange.  
   - `AnalysisCompleted` til **analysis.completed** fanout-exchange.  
8. **GraphGateway** lytter på disse exchanges og videresender events til GraphQL-subscriptions:  
   - `AnalysisStarted` → udløser `onAnalysisStarted` subscription.  
   - `AnalysisCompleted` → udløser `onAnalysisCompleted` subscription.  
9. **ClientConsole** modtager subscription-events over den eksisterende WebSocket og filtrerer efter `correlationId` i payloaden for at korrelere til sin forespørgsel.