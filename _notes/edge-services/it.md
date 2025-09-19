---
title: "Servizi Edge"
categories: [microservices]
tags: [architecture]
lang: it
locale: it
nav_order: 14
ref: note-edge-services
---
Esistono due tipi distinti di servizi edge:

**Inbound edge services** astraggono le dipendenze di terze parti. I contratti dei sistemi esterni cambiano frequentemente e chiamarli direttamente diffonde tale impatto su tutta la piattaforma. Introducendo un edge service, tutte le modifiche vengono gestite in un unico punto. Questo crea anche flessibilità se in futuro il fornitore esterno deve essere sostituito.

**Outbound edge services** gestiscono le esigenze specifiche dei client. Client diversi spesso richiedono variazioni nei payload o trasformazioni. Invece di spostare questa logica nell’API gateway o nei servizi core, gli outbound edge services gestiscono queste trasformazioni mantenendo comunque un’interfaccia coerente verso i client.

<small> Fonte: [LinkedIn Learning: Microservices Foundations](https://www.linkedin.com/learning/microservices-foundations-23469069?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>