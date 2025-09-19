---
title: "Servicetyper"
categories: [microservices]
tags: [architecture]
lang: da
locale: da
nav_order: 9
ref: note-service-types
---
Et microservices-system består typisk af en kombination af forskellige servicetyper. Tre almindelige mønstre er domænebaserede services, forretningsprocesbaserede services og atomiske transaktionsbaserede services. Sammen danner de systemets struktur og afspejler de forskellige niveauer, som services kan designes på.

**Domænebaserede microservices**

Domænebaserede microservices er det mest almindelige designmønster i microservices-arkitektur. Ved at bygge services omkring datadomæner bliver de mere skalerbare og nemmere at håndtere.

Opdelingen styres af selve domænet: servicen ejer sine data, anvender logik på dataene og eksponerer de nødvendige handlinger. Fokus er på klart ejerskab af domænemodellen, ikke på detaljerne i databaseskemaet.

Domain-driven design bruges ofte til at fastlægge servicegrænser. En afgrænset kontekst definerer, hvor en domænemodel gælder med en ensartet betydning. Ved at identificere disse kontekster og deres anvendelser kan et stort system opdeles i services, der matcher de reelle forretningsdomæner. Analyse af trafikmønstre hjælper med at forfine grænserne og reducere unødvendige kald på tværs af domæner og den latens, de skaber.

Processen med at bygge en domænebaseret service:

1. Modellér domænet  
2. Definér handlingerne  
3. Design datalageret til at understøtte det  

Dette holder servicen selvstændig og tæt knyttet til det domæne, den repræsenterer.

**Forretningsprocesbaserede microservices**

Forretningsprocesbaserede microservices giver et højere abstraktionsniveau ved at indkapsle specifikke forretningsfunktioner. De hjælper med at reducere duplikation, når processer spænder over flere domæner, hvilket forenkler arkitekturen og forbedrer skalerbarheden.

Disse services håndterer normalt ikke deres egne data. I stedet forbruger de de nødvendige domæneservices og fokuserer kun på orkestrering af logik. Denne adskillelse undgår at blande forretningsprocesser med dataejerskab og holder grænserne klare.

Processen med at bygge en forretningsproces-service:

1. Identificér processen, der skal eksponeres  
2. Identificér de datadomæner, den forbruger  
3. Definér API’erne med fokus på kontrakter frem for underliggende modeller  
4. Kapsl forretningsproceslogikken ind i sit eget modul  

Dette holder servicen uafhængig, genbrugelig og tilpasset højere forretningsoperationer.

**Atomiske transaktionsbaserede microservices**

Atomiske transaktionsbaserede microservices er nødvendige, når transaktioner skal spænde over flere datadomæner. Deres formål er at garantere ACID-kompatible operationer på tværs af services og sikre, at alle ændringer lykkes, eller at alle rulles tilbage.

Disse services er blokerende af natur: kaldet skal modtage enten en bekræftelse på succes eller en fejl. Selvom asynkrone mønstre findes, kræver de fleste anvendelser øjeblikkelige garantier. For enkelt-domæne transaktioner er denne specialisering unødvendig, da den underliggende services kan håndtere atomicitet internt.

Designovervejelser for atomiske transaktionsservices:

1. Bekræft om services virkelig behøver atomisk adfærd  
2. Brug en delt database på tværs af de involverede domæner (med forsigtighed, da dette øger koblingen)  
3. Definér transaktionsomfanget, inkl. rollback-betingelser  
4. Implementér med hurtig fejl- og rollback-mekanismer  

Disse services er komplekse og introducerer begrænsninger i distribution og skalerbarhed. De bør kun anvendes, når det er absolut nødvendigt, men når behovet opstår, skal de understøttes.

<small> Kilde: [LinkedIn Learning: Microservices Foundations](https://www.linkedin.com/learning/microservices-foundations-23469069?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>  
<small> Kilde: [LinkedIn Learning: Solving Microservices Problems with Patterns](https://www.linkedin.com/learning/microservices-design-patterns-23454771/solving-microservices-problems-with-patterns?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>