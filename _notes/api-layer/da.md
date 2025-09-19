---
title: "API-laget"
categories: [microservices]
tags: [architecture]
lang: da
locale: da
nav_order: 13
ref: note-api-layer
---
API-laget fungerer som en ren proxy, der eksponerer tjenester gennem ét indgangspunkt uden at tilføje klient-specifik logik. Det isolerer klienter fra at skulle kende IP-adressen eller porten på de tjenester, de forbruger.

Fra klientens perspektiv går alle forespørgsler til API-laget, som ruter dem til den rette tjeneste. Dette betyder, at klienten ikke påvirkes, hvis de underliggende microservices ændres, da API-laget håndterer eventuelle forskelle i intern kommunikation.

<small> Kilde: [LinkedIn Learning: Microservices Foundations](https://www.linkedin.com/learning/microservices-foundations-23469069?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>