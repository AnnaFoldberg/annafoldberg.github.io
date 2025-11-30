---
title: "Trackunit: Il Ruolo dell'Analysis Orchestrator"
categories: [trackunit, microservices]
tags: [architecture, messaging, events, commands, final]
lang: it
locale: it
nav_order: 49
ref: orchestration-architecture
---

**svc-analysis-orchestrator** è progettato per essere il punto centrale di coordinamento dell’intero flusso di analisi. Anche se l’implementazione attuale è semplificata per supportare lo sviluppo iniziale, l’architettura è pensata per una versione scalata in cui l’orchestrator gestisce il processo tramite command espliciti ed eventi dominio-specifici.

L’obiettivo è avere un unico servizio che sappia esattamente cosa deve succedere nell’analisi, quando deve succedere e quando l’intero processo è completato.

In questo modo ogni servizio di analisi può rimanere piccolo, specializzato e indipendente, mentre l’orchestrator gestisce il flusso complessivo.

##### Orchestrazione completa in un setup scalato

###### Commands

L’orchestrator invia command per avviare i vari passaggi:

- `analysis.recognition.request`: Avvia l’analisi dell’immagine in **svc-ai-vision-adapter**
- `analysis.comparison.request`: Avvia la comparazione dei risultati
- `analysis.metadata.request`: Avvia l’analisi o l’ordinamento dei metadata

###### Events

Ogni servizio emette eventi quando ha completato la propria parte:

- `analysis.started`: L’orchestrator ha ricevuto `tu.image.uploaded` e ha iniziato l’analisi.
- `recognition.completed`: Emesso da **svc-ai-vision-adapter**. Consumato da:
  - **graph-gateway**, che invia un corrispondente evento GraphQL a **trackunit-client**
  - **svc-analysis-orchestrator**, che registra lo stato e avvia il passaggio successivo
- `comparison.completed`: Emesso da **comparison-service**, consumato da gateway e orchestrator.
- `metadata.sorted`: Emesso da **metadata-service**. Consumato da:
  - **graph-gateway**
  - **svc-analysis-orchestrator**, che poi emette `analysis.completed`
- `analysis.completed`: Emesso da **svc-analysis-orchestrator** al termine dell’analisi. Consumato da:
  - **graph-gateway**
  - **svc-machine-storage**, che salva i dati della macchina.

Per ogni evento di successo esiste un evento `*.failed`. Questi vengono utilizzati da **svc-analysis-orchestrator** e **graph-gateway** per gestire errori e aggiornare **trackunit-client**.

##### Vantaggi di questa architettura

- I command sono point-to-point e gestiti da un servizio ben definito.  
- Gli eventi vengono pubblicati in broadcast, permettendo a più componenti di reagire.  
- **svc-analysis-orchestrator** controlla la sequenza ma non esegue l’analisi.  
- I servizi di analisi rimangono piccoli, specializzati e facilmente estendibili.  
- La persistenza è gestita esternamente, evitando blocchi nel workflow.  
- **trackunit-client** riceve solo eventi, mai command interni.  
- L’architettura permette di scalare servizi e fasi di analisi in modo indipendente.