---
title: "Trackunit Prototype: Analysis Flow"
categories: [trackunit, microservices]
tags: [rabbitmq, graphql, architecture]
lang: en
locale: en
nav_order: 15
ref: analysis-flow
---

1. **ClientConsole** opens a WebSocket to `/graphql` via Kong Gateway → oauth2-proxy → GraphGateway.  
2. **ClientConsole** starts a subscription filtered by `correlationId` or prepares to resubscribe once it has one.  
3. **ClientConsole** calls the `requestAnalysis` GraphQL mutation, passing an `objectKey`.  
4. **GraphGateway** receives the mutation, generates a `CorrelationId`, and returns it as `AnalysisRequestPayload(correlationId)` in the mutation payload.  
5. **GraphGateway** publishes a `RequestAnalysis` command to RabbitMQ (exchange: `analysis.commands`, routing key: `analysis.request`).  
6. **SvcAnalysisOrchestrator** consumes the `RequestAnalysis` command from its queue (orchestrator.analysis.commands) and simulates processing.  
7. As work progresses, the orchestrator publishes events:  
    - `AnalysisStarted` to the **analysis.started** fanout exchange.  
    - `AnalysisCompleted` to the **analysis.completed** fanout exchange.  
8. **GraphGateway** listens to those event exchanges and bridges them to GraphQL subscriptions:  
    - `AnalysisStarted` → triggers `onAnalysisStarted` subscription.  
    - `AnalysisCompleted` → triggers `onAnalysisCompleted` subscription.  
9. **ClientConsole** receives the subscription events over the existing WebSocket and filters by `correlationId` in the event payload to correlate to its request.  
