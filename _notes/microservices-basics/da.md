---
title: "Grundlæggende om microservices"
categories: [microservices]
tags: [architecture, tools]
lang: da
locale: da
weight: 1
ref: note-microservices-basics
---
Hver microservice håndterer én forretningsfunktion fra start til slut uafhængigt af andre microservices. De kommunikerer med hinanden gennem letvægts, fælles protokoller såsom HTTP eller message queues. Dermed muliggør de brugen af forskellige programmeringssprog til hver mikroservice.  
Det er dog vigtigt at være opmærksom på, at jo flere microservices der er, desto større bliver kompleksiteten. For eksempel kan en enkelt microservice, der fejler i produktion, skabe en kædereaktion af nedbrud i andre microservices, og det kan være svært at identificere årsagen og rette fejlen hurtigt.

##### Værktøjer
- **Containerisering:** Hjælper med at udrulle microservices i minimalistiske, selvstændige miljøer (docker)
- **Containerorkestrering:** Håndterer containeres livscyklus (kubernetes)
- **Pipeline-automatisering:** CI/CD (GitLab)
- **Asynkron beskedhåndtering:** Adskiller microservices yderligere ved at tilbyde message brokers og queues (kafka)
- **Ydelsesovervågning:** Overvåger microservice-ydelse (prometheus)
- **Logning og audit:** Hjælper med at holde styr på alt, hvad der sker i systemet (datadog)