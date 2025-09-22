---
title: "Mutual TLS"
categories: [microservices, it-security]
tags: [mtls, certificates, service-mesh, encryption]
lang: it
locale: it
nav_order: 26
ref: note-mtls
---
In un modello zero-trust, ogni microservizio verifica l’identità di chi chiama e l’integrità dei dati ricevuti. I certificati digitali sono il modo standard per stabilire l’identità: contengono i dettagli dell’entità, una chiave pubblica e informazioni sull’autorità di certificazione emittente.

TLS protegge la comunicazione cifrando il traffico tra client e server, ma in genere autentica solo il server. Il client convalida il certificato del server rispetto alle autorità fidate, mentre il server non verifica il client.

![Panoramica TLS](../../../assets/images/notes/security-between-microservices/mtls/tls-one-way.png)

**Mutual TLS (mTLS)** estende questo schema richiedendo che **entrambi** i lati presentino certificati validi emessi da un’autorità fidata prima di stabilire il canale. In questo modo i microservizi possono identificarsi e fidarsi l’uno dell’altro prima di scambiarsi dati. mTLS è ampiamente usato per proteggere la comunicazione service-to-service e dovrebbe proteggere anche quella tra API gateway e microservizi. Senza un certificato fidato, persino un attaccante all’interno della rete non può chiamare un servizio.

![Panoramica mTLS](../../../assets/images/notes/security-between-microservices/mtls/mtls-handshake.png)

Gestire mTLS su larga scala introduce sfide. I microservizi spesso girano in container effimeri, quindi il provisioning e la rotazione dei certificati devono essere automatizzati. Gli orchestrator di container e i service mesh includono in genere funzionalità integrate per gestire questi compiti.

<small> Fonte: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>