---
title: "Trackunit: Scelta delle Repliche"
categories: [trackunit, kubernetes]
tags: [pods, replicas, scaling, final]
lang: it
locale: it
nav_order: 46
ref: pod-replicas
---

##### Servizi propri

- **graph-gateway:** Viene eseguito con `3` pod perché rappresenta il punto di ingresso nel sistema. Tre repliche offrono una maggiore disponibilità in caso di crash di un pod e una capacità migliore di gestire picchi di traffico.  
- **svc-analysis-orchestrator:** Viene eseguito con `2` pod perché è centrale per il flusso di analisi, ma non si trova direttamente nel request-path del client. Due repliche forniscono fault tolerance senza usare tante risorse quanto la gateway.

##### Componenti distribuiti con Helm

- **Kong**, **oauth2-proxy** e **RabbitMQ:** Vengono eseguiti con `1` pod ciascuno. Sono servizi di infrastruttura e cambiano raramente rispetto ai servizi propri del progetto. Aumentare il numero di repliche ne accrescerebbe la complessità senza apportare un valore significativo nell’attuale setup. Rimangono comunque single point of failure, poiché gran parte del sistema viene influenzata se uno di essi non è disponibile.

##### Riepilogo

Nel progetto Trackunit, in pratica `1` pod per servizio sarebbe sufficiente, perché il sistema è piccolo, gira in locale e serve un solo client. La maggior parte dei componenti è mutuamente dipendente, quindi un guasto in un servizio influisce inevitabilmente sull’intero sistema.

Il numero di repliche per **graph-gateway** e **svc-analysis-orchestrator** è quindi scelto principalmente per dimostrare come i servizi possano essere scalati in base al loro ruolo e alla loro criticità in un ambiente più simile alla produzione. Le scelte di scalabilità mostrano come aumentare disponibilità e robustezza per le parti del sistema in cui ha più senso, mantenendo al contempo l’infrastruttura semplice.