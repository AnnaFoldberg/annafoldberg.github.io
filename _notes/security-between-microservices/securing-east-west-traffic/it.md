---
title: "Protezione del Traffico East–West"
categories: [microservices, it-security]
tags: [api-security, jwt, tokens, access-control]
lang: it
locale: it
nav_order: 27
ref: note-east-west-traffic
---
I microservizi seguono il principio della single responsibility, il che rende critica la comunicazione sicura tra di essi. Il traffico east–west si riferisce alle chiamate service-to-service all’interno del sistema e solleva domande su come venga condivisa l’identità dell’utente e dove vengano applicate le policy di accesso.

Un anti-pattern comune è utilizzare un unico access token con uno scope condiviso tra i servizi. Ciò crea forte accoppiamento, indebolisce il principio del least privilege e rischia di esporre direttamente servizi sensibili.

Una strategia è che un servizio richieda il proprio access token tramite il client credentials grant prima di chiamare un altro servizio. Questo separa gli scope, ma il servizio a valle perde consapevolezza dell’utente finale e deve fidarsi del chiamante. Se compromesso, quel chiamante potrebbe agire per conto di qualsiasi utente.

Un approccio più solido è trasmettere l’identità dell’utente insieme all’access token, tipicamente tramite un JWT contenente claim sull’utente finale. Verificando la firma JWT, il servizio a valle può applicare le proprie decisioni di controllo degli accessi. Per proteggere i dati utente, i client ricevono spesso solo un reference token; al livello dell’API gateway, questo viene scambiato con un token strutturato contenente claim che viene passato internamente tra i servizi.

Queste tecniche consentono ai microservizi di stabilire il contesto utente senza uno stato di sessione condiviso. È necessario un design accurato per evitare che scope o claim accoppino i servizi troppo strettamente, mantenendo al contempo una collaborazione sicura.

<small> Fonte: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>