---
title: "Datamønstre"
categories: [microservices]
tags: [architecture, patterns]
lang: da
locale: da
nav_order: 11
ref: note-data-patterns
---
Datamønstre beskriver almindelige tilgange til at håndtere lagring og transaktioner i microservices. Valget af mønster påvirker skalerbarhed, konsistens og hvordan services interagerer med hinanden på datalagret. Nogle mønstre fokuserer på databasedesign, mens andre adresserer, hvordan dataændringer flyder gennem et distribueret system.

**Single Service Database**

Hver service har sin egen dedikerede database, som er tilpasset dens datadomæne. Når belastningen på en service stiger, skalerer dens database med. Denne isolation gør det muligt at skalere services og datalagre uafhængigt, eller endda efter region, uden at påvirke resten af systemet.

Begrænsningen opstår, når domæner er involveret i atomiske transaktioner. I disse tilfælde kan databasegrænserne være nødt til at være mindre detaljerede for at understøtte ACID-garantier.

**Shared Service Database**

Nogle gange deler services en database, ofte på grund af virksomhedskontrakter eller opstartsbegrænsninger. I dette mønster håndteres datadistribution af databasesystemet selv gennem skemaer, keyspaces eller andre logiske partitioner.

Hver service bør have sine egne legitimationsoplysninger og kun få adgang til det skema, den ejer. Korrekt segmentering muliggør en fremtidig migration til fuldt isolerede databaser, da den logiske adskillelse allerede findes.

**Command Query Responsibility Segregation (CQRS)**

CQRS adskiller læse- og skriveoperationer i forskellige modeller. Forespørgsler kan transformere eller aggregere data til visninger, der er optimeret til forbrug, mens kommandoer følger forretningsregler for tilstandsændringer.

Dette mønster er især nyttigt for opgavebaserede brugergrænseflader og hændelsesdrevne systemer. Det kræver dog omhyggeligt design, da dårlige implementeringer ofte fører til fejl. Når det gøres korrekt, giver det stærk fleksibilitet i distribuerede miljøer.

**Asynkron eventbehandling**

Nogle arbejdsgange er for lange eller komplekse til et enkelt blokerende API-kald. Asynkron hændelsesbehandling løser dette ved at afkoble forespørgslen fra behandlingen.

Service-API’et udløser en hændelse og kvitterer straks til klienten, mens det faktiske arbejde fortsætter i baggrunden gennem en række asynkrone handlinger. Denne model er særligt effektiv i distribuerede systemer, hvor langvarige transaktioner eller kædede operationer er nødvendige.

<small> Kilde: [LinkedIn Learning: Solving Microservices Problems with Patterns](https://www.linkedin.com/learning/microservices-design-patterns-23454771/solving-microservices-problems-with-patterns?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>