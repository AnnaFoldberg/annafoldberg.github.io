---
title: "2025-11-04: Conclusions from Project Meeting"
categories: [trackunit]
tags: [project, group, feed-forward]
lang: en
locale: en
nav_order: 13
ref: log-2025-11-04-project-meeting-conclusions
---

During the meeting, we discussed how our services should be connected. The following overview describes how I expect the data flow and interaction between the system components to appear in the final solution.  

##### Message Broker Bridge
Some services use RabbitMQ, while others use pure Kafka or Kafka via Redpanda.  
Therefore, I will implement a small service, **svc-messaging-bridge**, which acts as a two-way bridge between Kafka and RabbitMQ, ensuring that the consumers in each service remain unchanged.  

##### Image Upload
1. The **client** requests the **graph-gateway** to upload an image.  
2. The **graph-gateway** generates a unique `correlationId` and then calls a REST API endpoint in **tu-ingestion-service**.  
3. **tu-ingestion-service** publishes an event to a Kafka topic via Redpanda.  
4. **tu-storage-service** subscribes to this topic, consumes the event, and creates a new record in its associated database.  
5. **tu-ingestion-service** returns both a presigned `PutUrl` and an `objectKey` to the **graph-gateway**, which forwards them to the **client**.  
6. The **client** uploads the file directly to external storage using the provided `PutUrl`.  
7. Once the upload is complete, the **client** sends a confirmation back to **tu-ingestion-service** through the **graph-gateway**, which forwards the request to the service’s REST API endpoint.  
8. **tu-ingestion-service** publishes an event to a Kafka topic via Redpanda.  
9. **tu-storage-service** subscribes to this topic, consumes the event, and updates the `status` in its database to *uploaded*.  
10. **tu-ingestion-service** publishes the `ImageUploaded` event to a Kafka topic via Redpanda, including the `objectKey` in the payload.  

> Note: This flow is kept separate from **svc-analysis-orchestrator** to ensure that the upload process can run independently. This allows it to operate as a standalone workflow, unaffected if **svc-analysis-orchestrator** is temporarily unavailable.

##### Analysis Orchestration
1. **svc-messaging-bridge** consumes the `ImageUploaded` event from the Kafka topic via Redpanda and publishes it to a fanout RabbitMQ exchange.  
2. **svc-analysis-orchestrator** consumes the event from its RabbitMQ queue.  
3. **svc-analysis-orchestrator** publishes an `AnalysisStarted` event to the `analysis.started` fanout exchange in RabbitMQ.  
4. **graph-gateway** consumes the `analysis.started` event from its RabbitMQ queue and forwards it to the GraphQL subscription `onAnalysisStarted` to the **client** via the WebSocket connection.  
5. **svc-messaging-bridge** consumes the `analysis.started` event from its RabbitMQ queue and republishes it to a Kafka topic.  
6. **svc-ai-vision-adapter** subscribes to this topic, consumes the event, and starts the AI flow (see *AI Image Analysis*). When the flow completes, it publishes an event to a Kafka topic.  
7. **svc-messaging-bridge** consumes the event from the Kafka topic and republishes it to a RabbitMQ exchange.  
8. **svc-analysis-orchestrator** consumes the event from its RabbitMQ queue.  
9. **svc-analysis-orchestrator** publishes an `AnalysisCompleted` event to the `analysis.completed` fanout exchange via RabbitMQ.  
10. **graph-gateway** consumes the `analysis.completed` event from its RabbitMQ queue and forwards it to the GraphQL subscription `onAnalysisCompleted` to the **client** via WebSocket.  

> Note: Additional events, such as `AnalysisFailed`, can be implemented if the AI flow result is invalid.

##### AI Image Analysis
1. **svc-ai-vision-adapter** subscribes to a Kafka topic and consumes the event.  
2. **svc-ai-vision-adapter** publishes an event to a Kafka topic.  
3. **tu-media-access-service** subscribes to this topic, consumes the event, and *performs actions that give **svc-ai-vision-adapter** access to the image to be analyzed.*  
6. **svc-ai-vision-adapter** analyzes the image using the Google Cloud Vision AI service.  
7. **svc-ai-vision-adapter** publishes an event to a Kafka topic that includes the analysis result in the payload.  

##### Next Steps
A temporary direct connection between **tu-ingestion-service** and **svc-ai-vision-adapter** will be established to test whether the Kafka publish mechanism works correctly between services.  
Once this has been verified, **svc-analysis-orchestrator** will be inserted as an intermediary.  
The system will be demonstrated at the next product briefing.