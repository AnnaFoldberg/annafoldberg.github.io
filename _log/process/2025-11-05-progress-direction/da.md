---
title: "2025-11-05: Fremskridt og Retning"
categories: [microservices, kubernetes, it-security]
tags: [individual, process, feedback, feed-forward]
lang: da
locale: da
nav_order: 15
ref: log-2025-11-05-progress-direction
---

**Feedback**

I mit arbejde med Trackunit-projektet har jeg opbygget et microservices-miljø baseret udelukkende på Docker og docker-compose. Formålet i denne fase var at få alle services til at fungere sammen i et lokalt setup og forstå, hvordan de kommunikerer på tværs af containere.

Core-services på dette tidspunkt er **GraphGateway** og **SvcAnalysisOrchestrator**, som begge bygges via Dockerfiles og publiceres som images til GitHub Container Registry gennem et CI-workflow.

**GraphGateway** fungerer som API-facade for systemet. Den eksponerer et GraphQL-API (queries, mutations og subscriptions) og sender RequestAnalysis-kommandoer til RabbitMQ. Den modtager også hændelserne analysis.started og analysis.completed og videresender dem til subscriptions-felterne onAnalysisStarted og onAnalysisCompleted.

**SvcAnalysisOrchestrator** orkestrerer analyser. Den consumer RequestAnalysis-kommandoer fra RabbitMQ og publicerer analysis.started og analysis.completed events. I denne fase simulerer den selve analysen med en kort forsinkelse.

**InfraCore** samler alle services og infrastrukturkomponenter i ét docker-compose-miljø, så hele systemet kan startes samlet og kommunikere via et fælles Docker-netværk.

**ClientConsole** fungerer som midlertidig klient til at teste autentificerings- GraphQL-flowet og bekræfte, at beskeder sendes korrekt gennem systemet.

**Feed-forward**

Mit næste skridt bliver at flytte driften fra docker-compose til Kubernetes-manifester. Det vil give mulighed for skalering, bedre isolation af services, NetworkPolicies og rolling updates samt en mere robust og fleksibel driftsmodel.