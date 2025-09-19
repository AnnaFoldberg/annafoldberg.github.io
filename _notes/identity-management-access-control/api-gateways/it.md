---
title: "API Gateway"
categories: [microservices, it-security]
tags: [access-control, api-gateway, reverse-proxy, api-security]
lang: it
locale: it
nav_order: 19
ref: note-api-gateways
---
Un reverse proxy agisce come front-end attraverso il quale passa tutto il traffico verso uno o più server. Fornisce un unico punto di ingresso, nascondendo i dettagli dei server sottostanti e centralizzando le funzionalità nel gateway.

Nei microservizi, un API Gateway è un tipo comune di reverse proxy. Posizionato al bordo del deployment, garantisce che tutto il traffico verso i servizi fluisca attraverso un’unica interfaccia API. Questo crea un punto di accesso sicuro e protegge i client dai cambiamenti dietro il gateway.

Le funzionalità aggiuntive del gateway spesso includono request tracing, monitoraggio delle prestazioni e monetizzazione delle API. Sebbene alcune funzioni possano sovrapporsi a quelle delle piattaforme IAM, il reverse proxy rimane il meccanismo centrale per l’accesso controllato ai microservizi.

<small> Fonte: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>