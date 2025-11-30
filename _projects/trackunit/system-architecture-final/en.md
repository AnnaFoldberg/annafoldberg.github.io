---
title: "Trackunit: System Architecture"
categories: [trackunit, microservices]
tags: [architecture, infrastructure, final]
lang: en
locale: en
nav_order: 19
ref: system-architecture-final
---

## System Architecture
>**Note:** Only components listed under **InfraCore** are deployed in Kubernetes.

### Client Applications

- **[trackunit-client](https://github.com/Team-2-Devs/trackunit-client)**  
  **Owner:** Eske.  
  Official client application for the Trackunit project, built in Flutter for mobile. Handles authentication via Microsoft Entra ID and communicates with **graph-gateway** over GraphQL. Supports the `startUpload` and `confirmUpload` mutations and the `onAnalysisStarted` and `onAnalysisCompleted` subscriptions. Used for initiating and performing image uploads to **MinIO** and receiving real-time analysis updates.  
  Does not support TLS in the local development environment due to Android emulator networking restrictions preventing the mobile client from resolving the local DNS name `graphql.local` and trusting its self-signed certificate without extensive configuration.

- **[client-console](https://github.com/Team-2-Devs/client-console)**  
  Developer-focused console application used during development. Implements the same authentication flow and supports the same GraphQL mutations and subscriptions as **trackunit-client**.  
  Retained in the project as a reference client for validating TLS between the client and Kong.  

### Core Services

- **[graph-gateway](https://github.com/team-2-devs/graph-gateway)**  
  Microservice serving as the API facade and application gateway for the system. Provides a GraphQL API with queries, mutations, and subscriptions. Calls REST API `POST /uploads/start` and `POST /uploads/confirm` on **tu-ingestion-service**. Consumes `analysis.started` and `analysis.completed` events from RabbitMQ and forwards corresponding `analysis/started` and `analysis/completed` events to the onAnalysisStarted and onAnalysisCompleted GraphQL subscription fields.  

- **[svc-analysis-orchestrator](https://github.com/team-2-devs/svc-analysis-orchestrator)**  
  Microservice that orchestrates analysis. Consumes `tu.image.uploaded` and `tu.recognition.completed` events from RabbitMQ and publishes `analysis.started` and `analysis.completed` events to RabbitMQ.
  >**Note:** Although **svc-analysis-orchestrator** should be the central entry point for all analysis workflows, the current implementation temporarily allows **svc-ai-vision-adapter** to consume the `tu.image.uploaded` event directly from **tu-ingestion-service**. This was a decision by the team to simplify early development. In a future iteration, the architecture would be expected to align with the intended responsibility boundaries, with **svc-analysis-orchestrator** serving as the sole consumer of the `tu.image.uploaded` event and orchestrating the full analysis pipeline.  

- **[svc-messaging-bridge](https://github.com/team-2-Devs/svc-messaging-bridge)**  
  Microservice that bridges events from Kafka into RabbitMQ. It consumes events from Kafka topics and republishes them (`tu.image.uploaded` and `tu.recognition.completed`) to RabbitMQ. This allows each microservice to interact solely with its own message broker, without needing to manage cross-broker integration or compatibility concerns.

- **[svc-ai-vision-adapter](https://github.com/Team-2-Devs/svc-ai-vision-adapter)**  
  **Owner:** Marlen. Description from the repository documentation.  
  A microservice that receives an image via a presigned URL, processes it with AI provider, aggregates the results, and returns a summarized response to the orchestrator.  

- **[tu-ingestion-service](https://github.com/Team-2-Devs/tu-ingestion-service)**  
  **Owner:** Kenneth. Description from the repository documentation.  
  A microservice that manages media upload sessions, validates metadata, and requests presigned PUT URLs from Storage for client uploads.

- **[tu-media-access-service](https://github.com/Team-2-Devs/tu-media-access-service)**  
  **Owner:** Kenneth. Description from the repository documentation.  
  A microservice that authorizes internal media access and retrieves presigned GET URLs from Storage for downstream consumers.  

- **[tu-storage-service](https://github.com/Team-2-Devs/tu-storage-service)**  
  **Owner:** Kenneth. Description from the repository documentation.  
  A microservice that interfaces with object storage to issue presigned PUT/GET URLs and enforce upload and download policies.  

### Shared Components

- **[Messaging](https://github.com/team-2-devs/messaging)**  
  NuGet package containing shared `MessageContracts` and reusable RabbitMQ and Kafka interfaces and implementations.

### Infrastructure

- **InfraCore**  
  Kubernetes-based orchestration environment bundling microservices and infrastructure components, including **graph-gateway**, **svc-analysis-orchestrator**, **Kong Gateway**, **oauth2-proxy**, and **RabbitMQ**.

- **kong-kong**  
  API Gateway running in db-less mode. Routes `/graphql` requests to **oauth2-proxy**, which validates incoming tokens before forwarding them to **graph-gateway**.

- **oauth2-proxy**  
  Authentication proxy that validates Bearer tokens (JWT) issued by Microsoft Entra ID via OIDC.  
  Only requests with valid tokens are forwarded to **graph-gateway**.

- **rabbitmq**  
  Message broker for asynchronous communication.