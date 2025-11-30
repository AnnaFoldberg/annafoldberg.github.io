---
title: "Trackunit Prototype: Exchanges & Routing"
categories: [trackunit, microservices]
tags: [rabbitmq, architecture, prototype]
lang: en
locale: en
nav_order: 16
ref: exchanges-routing
---

- **`analysis.commands` (direct)**
  - **Routing key:** `analysis.request` (`Routes.RequestAnalysis`)
  - **Queue:** `orchestrator.analysis.commands`
  - **Publisher:** GraphGateway (mutation `RequestAnalysisAsync`)
  - **Consumer:** SvcAnalysisOrchestrator (worker `RabbitAnalysisWorker`)

- **`analysis.started` (fanout)**
  - **Publisher:** SvcAnalysisOrchestrator
  - **Consumers:** GraphGateway (bridges to GraphQL subscriptions via `RabbitToSubscriptions`, queue `graph.subs.started`)

- **`analysis.completed` (fanout)**
  - **Publisher:** SvcAnalysisOrchestrator
  - **Consumers:** GraphGateway (bridges to GraphQL subscriptions via `RabbitToSubscriptions`, queue `graph.subs.completed`)
