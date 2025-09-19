---
title: "Tipi di Servizi"
categories: [microservices]
tags: [architecture, patterns]
lang: it
locale: it
nav_order: 9
ref: note-service-types
---
Un sistema a microservizi consiste tipicamente in una combinazione di diversi tipi di servizi. Tre schemi comuni sono i servizi basati sul dominio, i servizi basati sui processi aziendali e i servizi basati sulle transazioni atomiche. Insieme, formano la struttura del sistema e riflettono i diversi livelli a cui i servizi possono essere progettati.

**Microservizi basati sul dominio**

I microservizi basati sul dominio sono lo schema di progettazione più comune nell’architettura a microservizi. Costruire servizi attorno ai domini di dati li rende più scalabili e facili da gestire.

La decomposizione è guidata dal dominio stesso: il servizio possiede i propri dati, applica logica a tali dati ed espone le azioni necessarie. L’attenzione è rivolta alla chiara proprietà del modello di dominio, non ai dettagli dello schema del database.

Il domain-driven design è spesso usato per decidere i confini dei servizi. Un contesto delimitato definisce dove un modello di dominio si applica con un significato coerente. Identificando questi contesti e i loro casi d’uso, un grande sistema può essere suddiviso in servizi che si allineano ai veri domini aziendali. Analizzare i modelli di traffico aiuta a perfezionare i confini, riducendo chiamate trans-domini non necessarie e la latenza che esse creano.

Il processo di costruzione di un servizio basato sul dominio:

1. Modellare il dominio  
2. Definire le azioni  
3. Progettare l’archivio dati per supportarlo  

Questo mantiene il servizio autonomo e strettamente allineato al dominio che rappresenta.

**Microservizi basati sui processi aziendali**

I microservizi basati sui processi aziendali forniscono un livello più alto di astrazione incapsulando specifiche funzionalità aziendali. Aiutano a ridurre la duplicazione quando i processi si estendono su più domini, semplificando l’architettura e migliorando la scalabilità.

Questi servizi di solito non gestiscono i propri dati. Invece, consumano i servizi di dominio necessari e si concentrano solo sull’orchestrazione della logica. Questa separazione evita di mescolare processi aziendali con la proprietà dei dati, mantenendo chiari i confini.

Il processo di costruzione di un servizio basato sui processi aziendali:

1. Identificare il processo da esporre  
2. Identificare i domini di dati che consuma  
3. Definire le API, concentrandosi sui contratti piuttosto che sui modelli sottostanti  
4. Incapsulare la logica di processo aziendale nel proprio modulo  

Questo mantiene il servizio indipendente, riutilizzabile e allineato alle operazioni aziendali di livello superiore.

**Microservizi basati su transazioni atomiche**

I microservizi basati su transazioni atomiche sono necessari quando le transazioni devono coprire più domini di dati. Il loro scopo è garantire operazioni conformi ad ACID attraverso i servizi, assicurando che tutte le modifiche abbiano successo o che tutte vengano annullate.

Questi servizi sono bloccanti per progettazione: il chiamante deve ricevere una conferma di successo o un errore. Sebbene esistano schemi asincroni, la maggior parte dei casi richiede garanzie immediate. Per le transazioni a dominio singolo, questa specializzazione non è necessaria, poiché il servizio sottostante può gestire l’atomicità internamente.

Considerazioni progettuali per i servizi di transazioni atomiche:

1. Confermare se il servizio necessita davvero di comportamento atomico  
2. Utilizzare un database condiviso tra i domini coinvolti (con cautela, poiché ciò aumenta l’accoppiamento)  
3. Definire l’ambito della transazione, comprese le condizioni di rollback  
4. Implementare con meccanismi di fail-fast e rollback  

Questi servizi sono complessi e introducono limitazioni nella distribuzione e scalabilità. Devono essere utilizzati solo quando strettamente necessario, ma quando il bisogno si presenta, devono essere supportati.

<small> Fonte: [LinkedIn Learning: Microservices Foundations](https://www.linkedin.com/learning/microservices-foundations-23469069?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>  
<small> Fonte: [LinkedIn Learning: Solving Microservices Problems with Patterns](https://www.linkedin.com/learning/microservices-design-patterns-23454771/solving-microservices-problems-with-patterns?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>