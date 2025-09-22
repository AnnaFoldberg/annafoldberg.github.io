---
title: "2025-09-18: Indsigter fra Guildmøde"
categories: [microservices, kubernetes]
tags: [guild, individual, reflection]
lang: da
locale: da
nav_order: 9
ref: log-2025-09-18-guild-meeting-insights
---
##### Microservices og Kubernetes  
**GraphQL Server og servicestruktur**  
Vi talte om, at det i et mindre system sandsynligvis giver mest mening at nøjes med en enkelt GraphQL-server, selvom det skaber et single point of failure. Flere servere vil tilføje yderligere kompleksitet og bliver derfor først relevante at overveje i større løsninger.  
Vi fastslog desuden, at det stadig giver mening at bevare analyseservicen, selvom GraphQL-serveren allerede står for at route klientkald videre til de rette microservices. Forskellen er, at hvor GraphQL-serveren alene håndterer selve routingen fra klienten, kan analyseservicen indeholde forretningslogik (f.eks. hvordan systemet skal reagere, hvis et billede mangler) som ikke bør ligge i GraphQL-serveren.  

**Autentificering med proxy**  
Jeg fik afklaret, at en OAuth-proxy kan være en fin løsning, når Kong OSS ikke understøtter OIDC direkte.  

**Dokumentation**  
Vi talte også om vigtigheden af dokumentation i et microservices-setup. Selvom gruppen ikke har brug for at kende alle tekniske detaljer, er det vigtigt, at de ved, hvilke kald og parametre der skal bruges. Her bør vi løbende arbejde på at gøre dokumentationen klar og tilgængelig, og samtidig selv bevare overblikket, når systemet udvikler sig.  

**Messaging og synkronitet**  
Vi talte også om message brokers, og hvordan de dels sikrer asynkron kommunikation, så afsender og modtager ikke skal være online på samme tid, og dels i et vist omfang kan bevare rækkefølgen af beskeder, f.eks. inden for en kø eller partition, selv når services midlertidigt er nede.