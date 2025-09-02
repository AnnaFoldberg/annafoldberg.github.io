---
title: "Idempotente operationer"
categories: [microservices, kubernetes]
tags: [idempotency, reliability, patterns, best-practices]
lang: da
locale: da
nav_order: 4
ref: note-idempotent-operations
---
En idempotent operation er en operation, der har den samme effekt uanset hvor mange gange du udfører den ⇒ sluttilstanden vil altid være den samme.  

##### Idempotent by design
- **GET:** Returnerer altid den samme information.  
- **DELETE:** Når noget først er slettet, sletter gentagne kald det ikke yderligere.  
- **PUT:** At sætte ressourcens tilstand gentagne gange holder den uændret.  

##### Ikke idempotent by design
- **POST:** Som standard opretter POST en ny ressource eller udløser en handling.  

##### Gør ikke-idempotente operationer idempotente
- **Idempotency key:** Hvis gentagelser skal være sikre (f.eks. betalinger eller ordreafgivelse), kan du gøre en POST idempotent ved at kræve en unik idempotency key.  
    - Klienten sender en unik nøgle med forespørgslen.  
    - Serveren sikrer, at samme nøgle = samme resultat.  
- **Upserts i stedet for inserts:** Brug PUT i stedet for POST. PUT sætter hele tilstanden, så hvis det gentages, opnås stadig samme sluttilstand.  
- **Tilstandstjek:** Før du skriver, kontroller om det allerede er udført.  

##### Hvad bør være idempotent i microservices
- **Muterende operationer, der kan blive gentaget:** Hvis netværket fejler, kan klienter/Kubernetes automatisk forsøge igen. Med message queues kan brokere også levere beskeder igen. Hvis operationen ikke er idempotent, kan gentagelser føre til dobbeltbooking, dobbeltbetaling eller duplikerede jobs.  
- **Infrastruktur-opgaver:** At genanvende den samme konfiguration (f.eks. et Kubernetes-manifest) bør efterlade systemet i samme tilstand uden at forårsage fejl.  

##### Hvad behøver ikke at være idempotent
- **POST-operationer, hvor dubletter er tilsigtede forretningshandlinger:** F.eks. skrivning til en audit log, oprettelse af chatbeskeder, at like et opslag osv. I disse tilfælde bør et dobbelt kald skabe to poster.  