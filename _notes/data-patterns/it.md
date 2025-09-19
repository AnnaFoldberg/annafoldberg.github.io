---
title: "Pattern di Dati"
categories: [microservices]
tags: [architecture, patterns]
lang: it
locale: it
nav_order: 11
ref: note-data-patterns
---
I pattern di dati descrivono approcci comuni alla gestione dello storage e delle transazioni nei microservizi. La scelta del pattern influisce sulla scalabilità, sulla coerenza e su come i servizi interagiscono tra loro a livello di dati. Alcuni pattern si concentrano sul design del database, mentre altri affrontano il flusso delle modifiche ai dati in un sistema distribuito.

**Single Service Database**

Ogni servizio ha il proprio database dedicato, allineato con il suo dominio di dati. Quando il carico su un servizio aumenta, anche il suo database scala di conseguenza. Questo isolamento rende possibile scalare servizi e archivi dati in modo indipendente, o persino per regione, senza influenzare il resto del sistema.

La limitazione si presenta quando i domini sono coinvolti in transazioni atomiche. In questi casi, i confini del database potrebbero dover essere meno granulari per supportare le garanzie ACID.

**Shared Service Database**

A volte i servizi condividono un database, spesso a causa di contratti aziendali o vincoli di startup. In questo pattern, la distribuzione dei dati è gestita dal motore del database stesso tramite schemi, keyspace o altre partizioni logiche.

Ogni servizio dovrebbe avere le proprie credenziali e accedere solo allo schema che possiede. Una corretta segmentazione consente una migrazione futura verso database completamente isolati, poiché la separazione logica esiste già.

**Command Query Responsibility Segregation (CQRS)**

CQRS separa le operazioni di lettura e scrittura in modelli diversi. Le query possono trasformare o aggregare dati in viste ottimizzate per il consumo, mentre i comandi seguono le regole aziendali per i cambiamenti di stato.

Questo pattern è particolarmente utile per interfacce utente basate su task e sistemi guidati da eventi. Tuttavia, richiede una progettazione attenta, poiché implementazioni scadenti spesso portano a fallimenti. Quando realizzato correttamente, fornisce una potente flessibilità negli ambienti distribuiti.

**Eventi Asincroni**

Alcuni flussi di lavoro sono troppo lunghi o complessi per una singola chiamata API bloccante. Gli eventi asincroni risolvono questo problema disaccoppiando la richiesta dall’elaborazione.

L’API del servizio attiva un evento e conferma immediatamente al client, mentre il lavoro effettivo procede in background attraverso una serie di azioni asincrone. Questo modello è particolarmente efficace nei sistemi distribuiti in cui sono richieste transazioni di lunga durata o operazioni concatenate.

<small> Fonte: [LinkedIn Learning: Solving Microservices Problems with Patterns](https://www.linkedin.com/learning/microservices-design-patterns-23454771/solving-microservices-problems-with-patterns?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>