---
title: "Trackunit Prototype: Systemdiagram"
categories: [trackunit, microservices]
tags: [architecture, infrastructure, rabbitmq]
lang: da
locale: da
nav_order: 13
ref: system-architecture
---

```text
                     ┌──────────────────────┐
                     │     ClientConsole    │
                     │     (console app)    │
                     └────────┬─────────────┘
      HTTP/WS :8000 (GraphQL) │ ↑ 
                call mutation │ ╎ receive subscription events via WebSocket :8000
       requestAnalysis(input) ↓ ╎ (AnalysisStarted / AnalysisCompleted)
                      ┌─────────┴─────────┐
                      │   Kong Gateway    │
                      │   (API Gateway)   │
                      └───────┬───────────┘
        HTTP :4180 (internal) │ ↑ 
      foward mutation request │ ╎ forward subscription events via WebSocket :8000
       requestAnalysis(input) ↓ ╎ (AnalysisStarted / AnalysisCompleted)
                    ┌───────────┴───────────────┐
                    │       oauth2-proxy        │
                    │ (JWT validation via OIDC) │
                    └─────────┬─────────────────┘
        HTTP :8080 (internal) │ ↑ 
     receive mutation request │ ╎ publish subscription events via WebSocket :8000
       requestAnalysis(input) ↓ ╎ (AnalysisStarted / AnalysisCompleted)
                 ┌──────────────┴────────────────┐
                 │        GraphGateway           │
                 │      (GraphQL Server)         │
                 │  - handles requestAnalysis    │
                 │  - RabbitToSubscriptions      │
                 │    bridges RabbitMQ → GraphQL │
                 └─────────────┬─────────────────┘
                    AMQP :5672 │ △ 
               publish command │ ╎ receive events
             (RequestAnalysis) │ ╎ (AnalysisStarted / AnalysisCompleted)
                               ▼ ╎
         ┌───────────────────────┴────────────────────────────────────────────┐
         │                 RabbitMQ                                           │
         │  Exchanges: analysis.commands   (direct)                           │
         │             analysis.started    (fanout)                           │
         │             analysis.completed  (fanout)                           │
         │  Queues:    orchestrator.analysis.commands ← bind request.analysis │
         │             graph.subs.started             ← bind (fanout)         │
         │             graph.subs.completed           ← bind (fanout)         │
         └────────────────────┬───────────────────────────────────────────────┘
                   AMQP :5672 │ △
              receive command │ ╎ publish events
            (RequestAnalysis) │ ╎ (AnalysisStarted / AnalysisCompleted)
                              ▼ ╎
                 ┌──────────────┴───────────────┐
                 │    SvcAnalysisOrchestrator   │
                 │  - handles RequestAnalysis   │
                 │  - publishes start/completed │
                 └──────────────────────────────┘
```
> Note: Forenklet for klarhed.