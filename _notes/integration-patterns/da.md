---
title: "Integrationsmønstre"
categories: [microservices]
tags: [architecture, patterns]
lang: da
locale: da
nav_order: 10
ref: note-integration-patterns
---
Integrationsmønstre beskriver almindelige måder at strukturere kommunikation mellem microservices og klienter. De giver strategier til at rute, aggregere eller isolere forespørgsler afhængigt af systemets behov.

**Gateway Pattern**

Gateway-mønstret fungerer som indgangspunktet for klientkommunikation. Det giver en buffer mellem klientbehov og de underliggende services, isolerer systemet og eksponerer veldefinerede API’er.

En gateway kan videresende forespørgsler, dekorere payloads og udføre simple aggregeringer. Dette undgår at skubbe kompleksitet ud til klienten, samtidig med at gatewayen forbliver enkel.

Designstrategi for gateways:

1. Definér kontrakter  
2. Eksponér API’er gennem gatewayen  
3. Anvend streng versionskontrol  
4. Implementér gateway-komponenten  

**Process Aggregator Pattern**

Proces-aggregator-mønstret bruges, når flere forretningsprocesser skal kaldes sammen og kombineres i ét enkelt svar. I stedet for at kræve, at hver klient håndterer flere kald og replikerer logik, leverer aggregator-servicen ét API-endpoint.

Selvom det er nyttigt, kan aggregators blive flaskehalse eller skabe blokerende kald, hvis de ikke designes omhyggeligt. De bør kun bruges, hvor det kombinerede svar virkelig er nødvendigt.

Trin til at designe en aggregator:

1. Bestem de forretningsprocesser, der skal kombineres  
2. Definér procesreglerne  
3. Opret en ny model til det sammensatte payload  
4. Implementér API’et baseret på modellen  
5. Forbind servicen og anvend proceslogikken  

**Edge Pattern**

Edge-mønstret adresserer skalerbarhed og klient-specifikke behov. I modsætning til en gateway, som betjener alle klienter, fokuserer en edge-service på en enkelt klient eller en lille gruppe klienter med unikke krav.

Ved at anvende gateway-lignende transformationer ved kanten undgår systemet at overbelaste hoved-gatewayen og opnår fleksibilitet til nye klientintegrationer.

Designstrategi for edge-services:

1. Identificér klientbehov og begrænsninger  
2. Byg kontrakter  
3. Implementér kontrakter  
4. Vedligehold servicen så længe klienten understøttes  

**Edge vs. Gateway**

- **Edge:** Retter sig mod individuelle klienter, anvender gateway-mønstre på en klient-specifik måde, mere skalerbar og fleksibel.  
- **Gateway:** Centralt indgangspunkt for alle klienter, enklere at vedligeholde, men mindre tilpasset unikke klientbehov.  

<small> Kilde: [LinkedIn Learning: Solving Microservices Problems with Patterns](https://www.linkedin.com/learning/microservices-design-patterns-23454771/solving-microservices-problems-with-patterns?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>