---
title: "2025-11-04: Konklusioner fra Projektmøde"
categories: [trackunit]
tags: [project, group, feed-forward]
lang: da
locale: da
nav_order: 13
ref: log-2025-11-04-project-meeting-conclusions
---

Under mødet vendte vi, hvordan vores services skal kobles sammen. Nedenstående gennemgang beskriver, hvordan jeg forventer, at dataflowet og interaktionen mellem systemets komponenter kommer til at se ud i den endelige løsning.  

##### Message broker bridge
Nogle services bruger RabbitMQ, mens andre bruger ren Kafka eller Kafka via Redpanda. Derfor vil jeg implementere en mindre service, **svc-messaging-bridge**, der fungerer som en to-vejs bridge mellem Kafka og RabbitMQ, så consumers i hver service forbliver uændrede.  

##### Upload af billede
1. **Client** anmoder **graph-gateway** om at uploade et billede.  
2. **graph-gateway** genererer et unikt `correlationId` og kalder derefter et REST API-endpoint i **tu-ingestion-service**.  
3. **tu-ingestion-service** publicerer et event på et Kafka topic via Redpanda.
4. **tu-storage-service** abonnerer på dette topic og consumer eventet, og opretter en ny record i dens tilknyttede database.
5. **tu-ingestion-service** returnerer både en presigned `PutUrl`og en `objectKey` til **graph-gateway**, som videresender det til **client**.  
6. **Client** uploader filen direkte til en ekstern storage via den angivne `PutUrl`.  
7. Når uploaden er fuldført, sender **client** en confirmation tilbage til **tu-ingestion-service** gennem **graph-gateway**, som videresender anmodningen til servicens REST API-endpoint.  
8. **tu-ingestion-service** publicerer et event på et Kafka topic via Redpanda.
9. **tu-storage-service** abonnerer på dette topic og consumer eventet, og opdaterer `status` i den tilknyttede database til *uploaded*.
10. **tu-ingestion-service** publicerer eventet `ImageUploaded` på et Kafka topic via Redpanda, der blandt andet indeholder `objectKey` i sin payload.  

>Note: Dette flow holdes adskilt fra **svc-analysis-orchestrator** for at sikre, at uploadprocessen kan køre uafhængigt. På den måde fungerer det som et selvstændigt workflow, der ikke påvirkes, hvis **svc-analysis-orchestrator** er midlertidigt nede.

##### Analyseorkestrering
1. **svc-messaging-bridge** consumer eventet `ImageUploaded` fra Kafka topic via Redpanda og publicerer til en fanout RabbitMQ exchange.
2. **svc-analysis-orchestrator** consumer eventet fra sin queue i RabbitMQ.
3. **svc-analysis-orchestrator** publicerer et `AnalysisStarted` event til `analysis.started` fanout exchangen i RabbitMQ.
4. **graph-gateway** consumer `analysis.started` eventet fra sin queue i RabbitMQ og videresender det til GraphQL subscription `onAnalysisStarted` til **client** via WebSocket-forbindelsen.  
5. **svc-messaging-bridge** consumer `analysis.started` eventet fra sin queue i RabbitMQ og publicerer det videre til et Kafka topic.  
6. **svc-ai-vision-adapter** abonnerer på dette topic og consumer eventet, og påbegynder AI-flowet (se *AI billedanalyse*). Når dette flow er færdigt, publicerer det et event på et Kafka topic.  
7. **svc-messaging-bridge** consumer eventet fra Kafka topic og publicerer det videre til et RabbitMQ exchange.  
8. **svc-analysis-orchestrator** consumer eventet fra sin queue i RabbitMQ.  
9. **svc-analysis-orchestrator** publicerer et `AnalysisCompleted` event til `analysis.completed` fanout exchangen via RabbitMQ.  
10. **graph-gateway** consumer `analysis.completed` eventet fra sin queue i RabbitMQ og videresender det til GraphQL subscription `onAnalysisCompleted` til **client** via WebSocket-forbindelsen.  

>Note: Der kan implementeres ekstra events, såsom AnalysisFailed, hvis resultatet fra AI-flowet ikke er valid.

##### AI billedanalyse
1. **svc-ai-vision-adapter** abonnerer på et Kafka topic og consumer eventet.
2. **svc-ai-vision-adapter** publicerer et event på et Kafka topic.
3. **tu-media-access-service** abonnerer på dette topic og consumer eventet, og *gør noget, der giver **svc-ai-vision-adapter** adgang til billedet, der skal analyseres*
6. **svc-ai-vision-adapter** analyserer billedet ved hjælp af Google Cloud Vision AI-tjenesten.
7. **svc-ai-vision-adapter** publicerer et event på et Kafka topic, der blandt andet indeholder resultatet af analysen i sin payload.

##### Næste skridt
En midlertidig direkte forbindelse mellem **tu-ingestion-service** og **svc-ai-vision-adapter** etableres for at teste om publish-mekanismen i Kafka fungerer korrekt mellem services. Når dette er verificeret, indsættes **svc-analysis-orchestrator** som mellemled.  
Systemet vil blive demonstreret til næste produktvejledning.