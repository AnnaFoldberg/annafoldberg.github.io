---
title: "2025-09-03: Indsigter fra guildmøder"
categories: [microservices, kubernetes, it-security]
tags: [guild, individual, reflection]
lang: da
locale: da
nav_order: 7
ref: log-2025-09-03-guild-meeting-insights
---
**Microservices og Kubernetes**  
På mødet drøftede vi blandt andet valget af databaser og overvejede, om PostgreSQL kunne være et godt valg i vores kontekst. PostgreSQL er en objektrelationel database, som både kan håndtere klassiske relationelle data og give mulighed for at lagre objekter direkte. Det rejser spørgsmålet, om vi vil udnytte denne fleksibilitet, eller om det snarere vil skabe unødig kompleksitet.  
Særligt i relation til microservices blev der peget på, at hver microservice typisk har sin egen database. Hvis PostgreSQL her bruges til at lagre objekter direkte, kan vi ende med at have det samme billede eller objekt gemt i flere databaser, hvilket skaber redundans og øger lagerforbruget. I en mere klassisk arkitektur med én fælles database kombineret med object storage ville man derimod kun gentage linket til objektet, ikke selve data. Refleksionen handler derfor om, hvorvidt PostgreSQL skal bruges som en fælles løsning til både relationelle og objektlignende data i microservices, eller om det i praksis er mere hensigtsmæssigt at fortsætte med relationelle databaser i kombination med dedikeret object storage.  

**IT-sikkerhed**  
Vi blev mindet om vigtigheden af at undersøge, hvilke regulativer der gælder inden for vores projekt- og sikkerhedsområde, og hvordan de kan påvirke vores arbejde og prioriteringer. Det handler ikke kun om den tekniske implementering, men også om at sikre, at vi kontinuerligt opererer inden for de lovmæssige rammer.