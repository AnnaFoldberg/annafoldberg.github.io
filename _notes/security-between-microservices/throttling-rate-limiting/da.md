---
title: "Throttling og Rate Limiting"
categories: [microservices, it-security]
tags: [rate-limiting, api-security, scalability]
lang: da
locale: da
nav_order: 30
ref: note-throttling-rate-limiting
---
Skalering er den første reaktion, når belastningen på en microservice vokser, og container-orkestratorer understøtter dette med auto-skalering. Men skalering har sine grænser: ressourceomkostninger, udtømt kapacitet på værten eller situationer, hvor skalering vil være skadeligt, f.eks. under denial-of-service-angreb. I sådanne tilfælde bliver det kritisk at kontrollere forbruget.

- **Throttling** styrer klientforbrug ved at begrænse trafikken, når grænser nås, og forhindrer fuldstændige nedbrud.  
- **Kvoter** definerer antallet af kald, der er tilladt over længere intervaller, f.eks. pr. dag eller måned, og er ofte knyttet til monetariserede API’er.  
- **Rate limiting** fokuserer på samtidige kald, normalt målt i transaktioner pr. sekund.  

Kvoter og throttling-strategier kan anvendes på forskellige niveauer. Nogle klienter, såsom interne applikationer, kan tildeles højere kvoter end tredjepartsklienter. Granularitet kan også anvendes: begrænsninger pr. bruger (baseret på slutbrugerens identitet) eller begrænsninger pr. operation (baseret på ressourceforbruget for hvert API-kald).

Disse teknikker beskytter microservices mod overbelastning og sikrer balanceret og bæredygtig brug på tværs af alle klienter.

<small> Kilde: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>