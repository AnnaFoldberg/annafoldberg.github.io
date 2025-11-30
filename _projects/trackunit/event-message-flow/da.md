---
title: "Trackunit: Event Message Flow Architecture"
categories: [trackunit, microservices]
tags: [architecture, messaging, events, flow, final]
lang: da
locale: da
nav_order: 50
ref: event-message-flow
---

![event-message-flow-architecture.png](../../../assets/images/projects/trackunit/event-message-flow/event-message-flow-architecture.png)

> **Tip:** Åbn diagrammet i en ny fane for at zoome ind og se detaljer.

Dette diagram viser kommunikationsflowet mellem klienten, pods i Kubernetes-clusteret og de eksterne services, som systemet integrerer med.

##### Upload af billede

1. **trackunit-client** sender `startUpload`-mutationen til **graph-gateway**.  
2. **graph-gateway** kalder REST API-endpointet `POST /uploads/start` i **tu-ingestion-service**.  
3. **tu-ingestion-service** returnerer både en presigned `PutUrl` og en `objectKey` til **graph-gateway**, som videresender det til **trackunit-client**.  
4. **trackunit-client** uploader filen direkte til **MinIO** via `PutUrl`.  
5. Når uploaden er fuldført, sender **trackunit-client** `confirmUpload` til **graph-gateway**, som kalder `POST /uploads/confirm` i **tu-ingestion-service**.  
6. **tu-ingestion-service** publicerer eventet `tu.image.uploaded` på Kafka topic via Redpanda.

> **Note:** Upload-flowet holdes adskilt fra orchestreringen, så upload ikke påvirkes, hvis **svc-analysis-orchestrator** er nede.

##### AI-billedanalyse

1. **svc-ai-vision-adapter** consumer eventet fra Kafka-topic `tu.image.uploaded`.  
2. Adapteren henter billedet via **tu-media-access-service**.  
3. Adapteren analyserer billedet med Google Cloud Vision.  
4. Den publicerer eventet `tu.recognition.completed` på Kafka-topic.

##### Analyseorkestrering

1. **svc-messaging-bridge** consumer `tu.image.uploaded` fra Kafka og publicerer det videre til fanout exchange `tu.image.uploaded` i RabbitMQ.  
2. **svc-analysis-orchestrator** consumer eventet fra queue `orchestrator.image-uploaded` og udsender `AnalysisStarted` til exchange `analysis.started`.  
3. **graph-gateway** consumer eventet fra `graph.subs.started` og videresender det via GraphQL subscription `onAnalysisStarted` til **trackunit-client**.  
4. **svc-ai-vision-adapter** færdiggør analysen og publicerer `tu.recognition.completed`.  
5. **svc-messaging-bridge** sender eventet videre til exchange `tu.recognition.completed` i RabbitMQ.  
6. **svc-analysis-orchestrator** consumer eventet og udsender `AnalysisCompleted` til exchange `analysis.completed`.  
7. **graph-gateway** consumer eventet fra `graph.subs.completed` og sender det videre som `onAnalysisCompleted` til klienten.

> **Note:** Selvom orchestratoren bør være det eneste indgangspunkt, tillader den nuværende implementation, at **svc-ai-vision-adapter** også consumer `tu.image.uploaded` direkte fra Kafka. Dette var en midlertidig forsimplelse i udviklingsfasen. En senere iteration vil rette dette.