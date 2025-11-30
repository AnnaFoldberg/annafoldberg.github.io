---
title: "Trackunit: System Architecture"
categories: [trackunit, microservices]
tags: [architecture, infrastructure, final]
lang: da
locale: da
nav_order: 19
ref: system-architecture-final
---

## System Architecture
>**Note:** Kun komponenter listet under **InfraCore** er deployeret i Kubernetes.

### Client Applications

- **[trackunit-client](https://github.com/Team-2-Devs/trackunit-client)**  
  **Ansvarlig:** Eske.  
  Officiel klientapplikation til Trackunit-projektet, bygget i Flutter til mobile enheder. Håndterer autentifikation via Microsoft Entra ID og kommunikerer med **graph-gateway** over GraphQL. Understøtter mutationerne `startUpload` og `confirmUpload` samt subscription-felterne `onAnalysisStarted` og `onAnalysisCompleted`. Bruges til at initiere og udføre billeduploads til **MinIO** og modtage realtidsopdateringer fra analyseprocessen.  
  Understøtter ikke TLS i det lokale udviklingsmiljø på grund af netværksbegrænsninger i Android-emulatoren, som forhindrer klienten i at resolve det lokale DNS-navn `graphql.local` og stole på det selvsignerede certifikat uden omfattende konfiguration.

- **[client-console](https://github.com/Team-2-Devs/client-console)**  
  Konsolapplikation målrettet udviklere, brugt under udvikling. Implementerer samme autentifikationsflow og understøtter de samme GraphQL-mutationer og -subscriptions som **trackunit-client**.  
  Beholdes i projektet som referenceklient til validering af TLS mellem klient og Kong.  

### Core Services

- **[graph-gateway](https://github.com/team-2-devs/graph-gateway)**  
  Microservice, der fungerer som API-facade og applikationsgateway for systemet. Leverer et GraphQL API med queries, mutations og subscriptions. Kalder REST-endepunkterne `POST /uploads/start` og `POST /uploads/confirm` på **tu-ingestion-service**. Consumer `analysis.started` og `analysis.completed` events fra RabbitMQ og videresender de tilsvarende `analysis/started` og `analysis/completed` events til felterne onAnalysisStarted og onAnalysisCompleted i GraphQL-subscriptions.  

- **[svc-analysis-orchestrator](https://github.com/team-2-devs/svc-analysis-orchestrator)**  
  Microservice, der orkestrerer analyseflowet. Consumer events `tu.image.uploaded` og `tu.recognition.completed` fra RabbitMQ og publicerer `analysis.started` og `analysis.completed` events til RabbitMQ.  
  >**Note:** Selvom **svc-analysis-orchestrator** bør være det centrale indgangspunkt for alle analyseworkflows, tillader den nuværende implementation midlertidigt **svc-ai-vision-adapter** at consume `tu.image.uploaded` eventet direkte fra **tu-ingestion-service**. Dette var en team-beslutning for at forenkle den tidlige udvikling. I en fremtidig iteration forventes arkitekturen at blive justeret, så **svc-analysis-orchestrator** er den eneste consumer af `tu.image.uploaded` og orkestrerer hele analysepipeline.

- **[svc-messaging-bridge](https://github.com/team-2-devs/svc-messaging-bridge)**  
  Microservice, der brokobler events fra Kafka til RabbitMQ. Consumer Kafka-events og republiserer dem (`tu.image.uploaded` og `tu.recognition.completed`) til RabbitMQ. Dette gør det muligt for hver microservice kun at interagere med sin egen message broker uden at håndtere integration eller kompatibilitet på tværs af brokers.

- **[svc-ai-vision-adapter](https://github.com/Team-2-Devs/svc-ai-vision-adapter)**  
  **Ansvarlig:** Marlen. Beskrivelse fra repository-dokumentationen.  
  Microservice, der modtager et billede via en presigned URL, behandler det via AI-provider, samler resultaterne og returnerer et opsummeret svar til orkestratoren.  

- **[tu-ingestion-service](https://github.com/Team-2-Devs/tu-ingestion-service)**  
  **Ansvarlig:** Kenneth. Beskrivelse fra repository-dokumentationen.  
  Microservice, der håndterer medie-uploadsessioner, validerer metadata og anmoder om presigned PUT-URLs fra Storage til klientuploads.

- **[tu-media-access-service](https://github.com/Team-2-Devs/tu-media-access-service)**  
  **Ansvarlig:** Kenneth. Beskrivelse fra repository-dokumentationen.  
  Microservice, der autoriserer intern medieadgang og henter presigned GET-URLs fra Storage til downstream-forbrugere.  

- **[tu-storage-service](https://github.com/Team-2-Devs/tu-storage-service)**  
  **Ansvarlig:** Kenneth. Beskrivelse fra repository-dokumentationen.  
  Microservice, der integrerer med object storage for at udstede presigned PUT/GET-URLs og håndhæve upload- og download-politikker.  

### Shared Components

- **[Messaging](https://github.com/team-2-devs/messaging)**  
  NuGet-pakke, der indeholder fælles `MessageContracts` samt genanvendelige interfaces og implementationer til RabbitMQ og Kafka.

### Infrastructure

- **InfraCore**  
  Kubernetes-baseret orkestreringsmiljø, der samler microservices og infrastrukturkomponenter, herunder **graph-gateway**, **svc-analysis-orchestrator**, **Kong Gateway**, **oauth2-proxy** og **RabbitMQ**.

- **kong-kong**  
  API Gateway, der kører i db-less mode. Dirigerer `/graphql`-forespørgsler til **oauth2-proxy**, som validerer indgående tokens, før de videresendes til **graph-gateway**.

- **oauth2-proxy**  
  Autentifikations-proxy, der validerer Bearer tokens (JWT) udstedt af Microsoft Entra ID via OIDC.  
  Kun forespørgsler med gyldige tokens videresendes til **graph-gateway**.

- **rabbitmq**  
  Message broker til asynkron kommunikation.