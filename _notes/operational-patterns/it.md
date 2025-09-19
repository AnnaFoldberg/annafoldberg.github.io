---
title: "Pattern Operativi"
categories: [microservices]
tags: [microservices, architecture, operations]
lang: it
locale: it
nav_order: 12
ref: note-operational-patterns
---
I pattern operativi descrivono le pratiche che mantengono un sistema a microservizi osservabile, gestibile e affidabile in produzione.

**Log Aggregation Pattern**

I log sono una delle fonti più preziose di dati operativi. Con i microservizi, i log sono sparsi tra molti servizi, quindi devono essere aggregati in un unico flusso.

La coerenza è fondamentale: struttura, formato e tassonomia dei log dovrebbero essere condivisi tra i servizi, anche in un ambiente poliglotta. I log strutturati riducono l’overhead di parsing e gli identificatori di traccia devono essere iniettati ovunque nello stesso modo per ricostruire gli stack di chiamate.

**Metrics Aggregation Pattern**

Le metriche forniscono approfondimenti a livello di sistema con meno overhead rispetto ai log. Librerie standard e formati diffusi rendono la raccolta delle metriche disponibile in più linguaggi.

Il monitoraggio efficace dipende da tassonomia e visualizzazione. Le dashboard dovrebbero essere costruite sia a livello alto che dettagliato, arricchite con eventi come deployment e allarmi collegati a runbook. L’aggregazione delle metriche consente di valutare la salute del sistema senza leggere i log grezzi.

**Tracing Pattern**

Il tracing ricrea lo stack di chiamate tra i confini dei servizi. Un identificatore di traccia viene iniettato al bordo del sistema e propagato attraverso tutte le chiamate, idealmente fino al database.

Questo identificatore dovrebbe essere incluso in ogni messaggio di log, consentendo la correlazione tra log e trace. Il tracing basato su standard, combinato con strumenti di visualizzazione, rende diagnosticabili i flussi di chiamata distribuiti in un modo che i log monolitici non permettono.

**Configurazione Esterna**

Esternalizzare la configurazione migliora la flessibilità e il troubleshooting. I valori di configurazione dovrebbero essere iniettati o recuperati piuttosto che incorporati nel codice, con strumenti e convenzioni di denominazione coerenti.

È meglio esternalizzare troppo che troppo poco, poiché una configurazione mancante può ostacolare le operazioni. Allo stesso tempo, i segreti devono essere protetti attentamente.

**Service Discovery**

Con la crescita del numero di servizi, la configurazione statica diventa ingestibile. Il service discovery fornisce un registro centrale in cui i servizi pubblicano i propri endpoint e le proprie capacità.

Altri servizi possono interrogare il registro per risolvere dinamicamente gli URI, evitando indirizzi hardcoded. Questo supporta la scalabilità a centinaia o migliaia di servizi senza configurazione manuale.

**Continuous Delivery**

La continuous delivery garantisce che il codice arrivi in produzione tramite pipeline completamente automatizzate. Questo include pubblicazione degli artefatti, test di integrazione, deployment e test di sicurezza.

Una volta in produzione, l’automazione continua tramite smoke test programmati. Nei microservizi, CI/CD è essenziale, poiché i deployment manuali non possono scalare con il numero di servizi.

**Documentazione**

Una documentazione chiara e coerente è fondamentale nei microservizi. OpenAPI è ampiamente utilizzato per i contratti RESTful e supporta sia lo sviluppo contract-first che code-first.

La documentazione dovrebbe essere esposta in un luogo centrale, includendo esempi di utilizzo delle API. Un archivio documentale coerente aiuta i team a orientarsi in un sistema complesso.

<small> Fonte: [LinkedIn Learning: Solving Microservices Problems with Patterns](https://www.linkedin.com/learning/microservices-design-patterns-23454771/solving-microservices-problems-with-patterns?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>