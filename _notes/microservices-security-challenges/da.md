---
title: "Sikkerhedsudfordringer i Microservices"
categories: [microservices, it-security]
tags: [authentication, attack-surface]
lang: da
locale: da
nav_order: 17
ref: note-microservice-security-challenges
---
**Sikkerhed i lagdelt arkitektur:**

- Begrænset angrebsflade  
- Trafikaflytning/interceptor  
- Delt sikkerhedskontekst  

**Microservice-arkitektur:**

- Ulemper:  
    - Bred angrebsflade  
    - Dynamisk angrebsflade (når en tjeneste skaleres, eller en ny tjeneste introduceres)  
    - Udfordrende autentificering  
    - Krydsende tillidsdomæner  
- Fordele:  
    - Forbedret tilgængelighed (beskyttelse mod DoS-angreb)  
    - Begrænset påvirkning ved brud  

<small> Kilde: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>