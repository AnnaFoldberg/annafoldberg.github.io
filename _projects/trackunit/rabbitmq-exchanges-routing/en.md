---
title: "Trackunit: RabbitMQ Exchanges & Routing"
categories: [trackunit, microservices]
tags: [rabbitmq, architecture, final]
lang: en
locale: en
nav_order: 21
ref: rabbitmq-exchanges-routing
---

- **`tu.image.uploaded` (fanout)**
  - **Publisher:** tu-ingestion-service
  - **Consumers:** svc-analysis-orchestrator (via `svc-messaging-bridge`, queue `orchestrator.image-uploaded`)

- **`tu.recognition.completed` (fanout)**
  - **Publisher:** svc-ai-vision-adapter
  - **Consumers:** svc-analysis-orchestrator (via `svc-messaging-bridge`, queue `orchestrator.recognition-completed`)

- **`analysis.started` (fanout)**
  - **Publisher:** svc-analysis-orchestrator
  - **Consumers:** graph-gateway (bridges to GraphQL subscriptions via `RabbitToSubscriptions`, queue `graph.subs.started`)

- **`analysis.completed` (fanout)**
  - **Publisher:** svc-analysis-orchestrator
  - **Consumers:** graph-gateway (bridges to GraphQL subscriptions via `RabbitToSubscriptions`, queue `graph.subs.completed`)