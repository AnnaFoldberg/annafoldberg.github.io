---
title: "Sfide di Sicurezza nei Microservizi"
categories: [microservices, it-security]
tags: [authentication, attack-surface]
lang: it
locale: it
nav_order: 17
ref: note-microservice-security-challenges
---
**Sicurezza in un’architettura a livelli:**

- Superficie di attacco limitata  
- Intercettore del traffico  
- Contesto di sicurezza condiviso  

**Architettura a microservizi:**

- Svantaggi:  
    - Ampia superficie di attacco  
    - Superficie di attacco dinamica (quando si scala un servizio o viene introdotto un nuovo servizio)  
    - Autenticazione complessa  
    - Attraversamento di domini di fiducia  
- Vantaggi:  
    - Maggiore disponibilità (protezione contro attacchi DoS)  
    - Impatto delle violazioni limitato  

<small> Fonte: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>