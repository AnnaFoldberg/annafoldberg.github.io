---
title: "Access Scenarios"
categories: [microservices, it-security]
tags: [access-control, api-gateway]
lang: it
locale: it
nav_order: 20
ref: note-access-scenarios
---
L’accesso a un microservizio avviene tramite un client. Nella maggior parte dei sistemi, le chiamate dei client sono instradate attraverso l’API Gateway, che centralizza l’accesso e il controllo.

I client differiscono nel modo in cui gestiscono le credenziali. **Public client**, come le single-page application che girano in un browser, non possono mantenere riservate le credenziali. **Confidential client**, come le applicazioni lato server, possono proteggerle in misura maggiore.

Anche la proprietà è importante. **First-party client** appartengono alla stessa organizzazione che ha costruito il servizio, mentre i **third-party client** sono esterni e richiedono controlli di sicurezza più rigorosi, poiché la loro postura non può essere garantita. Alcune organizzazioni applicano questi standard più severi a entrambi.

Infine, i client possono essere **interni** o **esterni**. Le applicazioni interne vengono eseguite all’interno del firewall aziendale, mentre quelle esterne si affacciano su Internet pubblico. Tuttavia, la posizione nella rete non dovrebbe mai ridurre i requisiti di sicurezza, poiché le minacce interne e i movimenti laterali rimangono possibili una volta che il perimetro è stato violato.

<small> Fonte: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>