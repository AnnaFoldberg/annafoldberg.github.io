---
title: "Piattaforme di Identity and Access Management"
categories: [microservices, it-security]
tags: [iam, access-control, tokens]
lang: it
locale: it
nav_order: 18
ref: note-iam-platforms
---
**Capacità di una piattaforma IAM:**

- **Autenticazione:** Supporta diverse strategie per verificare l’identità di utenti finali e client  
- **Gestione delle identità:** Gestione degli attributi di identità e dei privilegi di accesso di varie parti, inclusi utenti finali e client  
- **Implementazioni di standard:** Fornisce implementazioni per standard ampiamente adottati per controllo degli accessi, verifica delle identità e registrazione dei client  
- **Gestione dei token:** Offre funzionalità per emettere, revocare e ispezionare token in diversi formati di codifica.  

![Funzionalità IAM](../../../assets/images/notes/iam-platforms/iam-capabilities.png)

Le principali piattaforme si integrano con vari archivi di identità per autenticare gli utenti. Questi archivi contengono credenziali e permessi usati per il controllo degli accessi, che spaziano da database locali ad applicazioni aziendali o login social. Nei casi sensibili, l’autenticazione a più fattori aggiunge un secondo fattore, come un PIN monouso o dati biometrici.  

![IAM e Archivi di Identità](../../../assets/images/notes/iam-platforms/iam-identity-stores.png)

Queste soluzioni sono anche ottime fonti di implementazioni degli standard e protocolli di sicurezza utilizzati dai microservizi, come OAuth2, OpenID Connect e JSON Web Token.  

![Standard IAM](../../../assets/images/notes/iam-platforms/iam-standards.png)

L’immagine seguente illustra i **servizi di token di una piattaforma IAM**, mostrando come i token vengono emessi, rinnovati, scaduti, revocati, verificati, archiviati e formattati per supportare autenticazione e controllo degli accessi.  

![Servizi Token IAM](../../../assets/images/notes/iam-platforms/iam-token-services.png)

<small> Fonte: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>