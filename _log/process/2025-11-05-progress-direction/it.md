---
title: "2025-11-05: Progressi e Direzione"
categories: [microservices, kubernetes, it-security]
tags: [individual, process, feedback, feed-forward]
lang: it
locale: it
nav_order: 15
ref: log-2025-11-05-progress-direction
---

**Feedback**

Nel mio lavoro sul progetto Trackunit ho costruito un ambiente a microservizi basato interamente su Docker e docker-compose. L’obiettivo in questa fase era far funzionare tutti i servizi insieme in un setup locale e comprendere come comunicano tra i container.

I servizi core in questo momento sono **GraphGateway** e **SvcAnalysisOrchestrator**, entrambi compilati tramite Dockerfile e pubblicati come immagini nel GitHub Container Registry attraverso un workflow CI.

**GraphGateway** funge da facciata API del sistema. Espone una GraphQL API (query, mutation e subscription) e invia comandi `RequestAnalysis` a RabbitMQ. Inoltre riceve gli eventi `analysis.started` e `analysis.completed` e li inoltra ai campi di subscription `onAnalysisStarted` e `onAnalysisCompleted`.

**SvcAnalysisOrchestrator** orchestra le analisi. Consuma i comandi `RequestAnalysis` da RabbitMQ e pubblica gli eventi `analysis.started` e `analysis.completed`. In questa fase simula l’analisi vera e propria con un breve ritardo.

**InfraCore** raggruppa tutti i servizi e i componenti infrastrutturali in un unico ambiente docker-compose, permettendo all’intero sistema di avviarsi insieme e comunicare attraverso una rete Docker condivisa.

**ClientConsole** funge da client temporaneo per testare il flusso autenticazione–GraphQL e verificare che i messaggi vengano trasmessi correttamente attraverso il sistema.

**Feed-forward**

Il mio prossimo passo sarà spostare l’esecuzione da docker-compose ai manifest Kubernetes. Ciò consentirà scalabilità, una migliore isolazione dei servizi, NetworkPolicies, rolling update e un modello operativo più robusto e flessibile.