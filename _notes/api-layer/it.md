---
title: "Il livello API"
categories: [microservices]
tags: [architecture]
lang: it
locale: it
nav_order: 13
ref: note-api-layer
---
Il livello API funge da puro proxy, esponendo i servizi attraverso un unico punto di ingresso senza aggiungere logica specifica per il client. Isola i client dal dover conoscere l’indirizzo IP o la porta dei servizi che consumano.

Dal punto di vista del client, tutte le richieste passano attraverso il livello API, che le instrada al servizio appropriato. Ciò significa che il client non è influenzato se i microservizi sottostanti cambiano, poiché il livello API gestisce eventuali differenze di comunicazione interna.

<small> Fonte: [LinkedIn Learning: Microservices Foundations](https://www.linkedin.com/learning/microservices-foundations-23469069?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>