---
title: "API Gateways"
categories: [microservices, it-security]
tags: [access-control, api-gateway, reverse-proxy]
lang: da
locale: da
nav_order: 19
ref: note-api-gateways
---
En reverse proxy fungerer som en front-end, hvor al trafik til en eller flere servere passerer igennem. Den giver ét samlet indgangspunkt, skjuler detaljerne om de underliggende servere og centraliserer funktionalitet i gatewayen.

I microservices er en API gateway en almindelig type reverse proxy. Placeret ved kanten af deployment sikrer den, at al trafik til services flyder gennem ét API-interface. Dette skaber et sikkert adgangspunkt og beskytter klienter mod ændringer bag gatewayen.

Yderligere funktioner i gateways omfatter ofte request tracing, performance-overvågning og API-monetarisering. Selvom funktioner kan overlappe med IAM-platforme, forbliver reverse proxy den centrale mekanisme for kontrolleret adgang til microservices.

<small> Kilde: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>