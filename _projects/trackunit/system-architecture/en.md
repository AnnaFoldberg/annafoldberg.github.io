---
title: "Trackunit Prototype: System Architecture"
categories: [trackunit, microservices]
tags: [architecture, infrastructure, prototype]
lang: en
locale: en
nav_order: 8
ref: system-architecture
---

##### Core Services

- **[GraphGateway](https://github.com/team-2-devs/graph-gateway)**  
  Microservice serving as the API facade and application gateway for the system. Provides a GraphQL API with queries, mutations, and subscriptions. Publishes `RequestAnalysis` commands to RabbitMQ and forwards `analysis/started` and `analysis/completed` events to the `onAnalysisStarted` and `onAnalysisCompleted` GraphQL subscription fields.

- **[SvcAnalysisOrchestrator](https://github.com/team-2-devs/svc-analysis-orchestrator)**  
  Microservice that orchestrates analysis. Consumes `RequestAnalysis` commands from RabbitMQ and publishes `analysis.started` and `analysis.completed` event exchanges.

##### Shared Components

- **[Messaging](https://github.com/team-2-devs/messaging)**  
  NuGet package containing shared `MessageContracts` and reusable RabbitMQ publisher interfaces and implementations.

##### Infrastructure

- **[InfraCore](https://github.com/team-2-devs/infra-core)**
  Docker Compose-based orchestration environment bundling all services and infrastructure components, including **Kong Gateway**, **oauth2-proxy**, and **RabbitMQ**.

- **Kong Gateway**  
  API Gateway running in db-less mode. Routes `/graphql` requests to **oauth2-proxy**, which validates incoming tokens before forwarding them to **GraphGateway**.

- **oauth2-proxy**  
  Authentication proxy that validates Bearer tokens (JWT) issued by Microsoft Entra ID.  
  Only requests with valid tokens are forwarded to **GraphGateway**.

- **RabbitMQ**  
  Message broker for asynchronous communication.