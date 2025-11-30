---
title: "Trackunit: RabbitMQ Exchanges & Routing"
categories: [trackunit, microservices]
tags: [rabbitmq, architecture, final]
lang: it
locale: it
nav_order: 21
ref: rabbitmq-exchanges-routing
---

##### RabbitMQ: Exchanges & Routing
- **`tu.image.uploaded` (fanout)**
  - **Publisher:** tu-ingestion-service
  - **Consumer:** svc-analysis-orchestrator (tramite `svc-messaging-bridge`, coda `orchestrator.image-uploaded`)

- **`tu.recognition.completed` (fanout)**
  - **Publisher:** svc-ai-vision-adapter
  - **Consumer:** svc-analysis-orchestrator (tramite `svc-messaging-bridge`, coda `orchestrator.recognition-completed`)

- **`analysis.started` (fanout)**
  - **Publisher:** svc-analysis-orchestrator
  - **Consumer:** graph-gateway (collega agli abbonamenti GraphQL tramite `RabbitToSubscriptions`, coda `graph.subs.started`)

- **`analysis.completed` (fanout)**
  - **Publisher:** svc-analysis-orchestrator
  - **Consumer:** graph-gateway (collega agli abbonamenti GraphQL tramite `RabbitToSubscriptions`, coda `graph.subs.completed`)