---
title: "Trackunit: Event Message Flow Architecture"
categories: [trackunit, microservices]
tags: [architecture, messaging, events, flow, final]
lang: en
locale: en
nav_order: 50
ref: event-message-flow
---

![event-message-flow-architecture.png](../../../assets/images/projects/trackunit/event-message-flow/event-message-flow-architecture.png)

> **Tip:** Open the diagram in a new tab to zoom in and see details.

This diagram illustrates the communication flow between the client, the Kubernetes pods, and the external services integrated into the system.

##### Image upload

1. **trackunit-client** sends the `startUpload` mutation to **graph-gateway**.  
2. **graph-gateway** calls `POST /uploads/start` in **tu-ingestion-service**.  
3. **tu-ingestion-service** returns a presigned `PutUrl` and an `objectKey`, which **graph-gateway** forwards to **trackunit-client**.  
4. **trackunit-client** uploads the file directly to **MinIO** using `PutUrl`.  
5. After the upload completes, **trackunit-client** sends `confirmUpload`, which **graph-gateway** forwards to `POST /uploads/confirm`.  
6. **tu-ingestion-service** publishes the `tu.image.uploaded` event to Kafka via Redpanda.

> **Note:** The upload flow is intentionally isolated to ensure it operates independently of **svc-analysis-orchestrator**.

##### AI image analysis

1. **svc-ai-vision-adapter** consumes `tu.image.uploaded` from Kafka.  
2. It retrieves the image from **tu-media-access-service**.  
3. It analyzes the image using Google Cloud Vision.  
4. It publishes `tu.recognition.completed` to Kafka.

##### Analysis orchestration

1. **svc-messaging-bridge** consumes `tu.image.uploaded` and republishes it to RabbitMQ exchange `tu.image.uploaded`.  
2. **svc-analysis-orchestrator** consumes it and emits `AnalysisStarted` to exchange `analysis.started`.  
3. **graph-gateway** consumes the event and forwards it to **trackunit-client** via the `onAnalysisStarted` subscription.  
4. **svc-ai-vision-adapter** completes analysis and emits `tu.recognition.completed`.  
5. **svc-messaging-bridge** republishes it to exchange `tu.recognition.completed`.  
6. **svc-analysis-orchestrator** consumes it and emits `AnalysisCompleted` to exchange `analysis.completed`.  
7. **graph-gateway** consumes it and forwards it as `onAnalysisCompleted` to the client.

> **Note:** Although **svc-analysis-orchestrator** should be the single entry point for all analysis workflows, the current implementation allows **svc-ai-vision-adapter** to consume `tu.image.uploaded` directly from Kafka for early-development simplicity.