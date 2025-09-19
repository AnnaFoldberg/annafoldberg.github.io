---
title: "Edge Services"
categories: [microservices]
tags: [architecture]
lang: da
locale: da
nav_order: 14
ref: note-edge-services
---
Der findes to forskellige typer edge services:

**Inbound edge services** abstraherer tredjepartsafhængigheder. Eksterne systemkontrakter ændrer sig ofte, og hvis de kaldes direkte, spreder det konsekvenserne ud over hele platformen. Ved at introducere en edge service håndteres alle ændringer ét sted. Dette skaber også fleksibilitet, hvis den eksterne udbyder skal udskiftes i fremtiden.

**Outbound edge services** håndterer klient-specifikke behov. Forskellige klienter kræver ofte variationer i payloads eller transformationer. I stedet for at skubbe denne logik ind i API-gatewayen eller kerneservices, håndterer outbound edge services disse transformationer, mens de stadig giver en ensartet grænseflade til klienter.

<small> Kilde: [LinkedIn Learning: Microservices Foundations](https://www.linkedin.com/learning/microservices-foundations-23469069?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>