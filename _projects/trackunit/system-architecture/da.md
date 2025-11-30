---
title: "Trackunit Prototype: Systemarkitektur"
categories: [trackunit, microservices]
tags: [architecture, infrastructure]
lang: da
locale: da
nav_order: 8
ref: system-architecture
---

##### Core services

- **[GraphGateway](https://github.com/team-2-devs/graph-gateway)**  
  Microservice, der fungerer som API-facade og applikationsgateway for systemet. Leverer et GraphQL API med queries, mutations og subscriptions. Udsender `RequestAnalysis`-kommandoer til RabbitMQ og videresender `analysis/started` og `analysis/completed` events til felterne `onAnalysisStarted` og `onAnalysisCompleted` i GraphQL-subscriptions.

- **[SvcAnalysisOrchestrator](https://github.com/team-2-devs/svc-analysis-orchestrator)**  
  Microservice, der orkestrerer analyser. Consumer `RequestAnalysis`-kommandoer fra RabbitMQ og publicerer `analysis.started` og `analysis.completed` event exchanges.

##### Delte komponenter

- **[Messaging](https://github.com/team-2-devs/messaging)**  
  NuGet-pakke, der indeholder fælles `MessageContracts` og genanvendelige RabbitMQ publisher-interfaces og implementeringer.

##### Infrastruktur

- **[InfraCore](https://github.com/team-2-devs/infra-core)**  
  Docker Compose-baseret orkestreringsmiljø, der samler alle services og infrastrukturkomponenter, herunder **Kong Gateway**, **oauth2-proxy** og **RabbitMQ**.

- **Kong Gateway**  
  API Gateway, der kører i db-less mode. Dirigerer `/graphql`-forespørgsler til **oauth2-proxy**, som validerer indgående tokens, før de videresendes til **GraphGateway**.

- **oauth2-proxy**  
  Autentifikations-proxy, der validerer Bearer-tokens (JWT) udstedt af Microsoft Entra ID.  
  Kun forespørgsler med gyldige tokens videresendes til **GraphGateway**.

- **RabbitMQ**  
  Message broker til asynkron kommunikation.