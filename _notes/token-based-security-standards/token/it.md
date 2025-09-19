---
title: "Token"
categories: [microservices, it-security]
tags: [tokens, jwt, authentication, authorization]
lang: it
locale: it
nav_order: 21
ref: note-tokens
---
I microservizi si basano sui token per stabilire l’identità e applicare il controllo degli accessi. A differenza dei monoliti, non esiste una sessione condivisa tra i servizi per memorizzare lo stato dell’utente, e passare le credenziali tra i servizi non è sicuro. I token risolvono questo problema trasportando informazioni di identità e accesso senza esporre le credenziali dell’utente.

![Token](../../../assets/images/notes/token-based-security-standards/token/token-overview.png)

A livello generale, i token si dividono in due formati:

- **Reference token**: Stringhe opache senza significato incorporato. Fungono da identificatori che puntano a metadati memorizzati all’interno della piattaforma IAM.  
- **Structured token**: Contengono metadati sull’evento di autenticazione e sull’utente. I dati sono memorizzati come coppie chiave–valore chiamate *claims*, raggruppate in un *claim set*.  

Uno standard comune per gli structured token è il **JSON Web Token (JWT)**. I JWT sono composti da tre parti:

1. **Header**: Specifica come il token è protetto crittograficamente.  
2. **Payload**: Contiene i claims relativi all’utente o all’evento di autenticazione.  
3. **Signature**: Garantisce l’integrità verificando che il token non sia stato modificato dalla sua creazione.  

I JWT consentono la trasmissione sicura dei dettagli relativi a utente e autenticazione tra client e servizi, rendendoli una scelta ampiamente utilizzata nelle architetture a microservizi.

**Tipi di token**

- **Access token:** Consente al possessore del token di accedere a un’API  
- **Refresh token:** Utilizzato per ottenere un nuovo access token dopo la scadenza di quello originale  
- **ID token:** JWT contenente informazioni sull’evento di autenticazione e sull’identità dell’utente  

**Ciclo di vita del token**

Il ciclo di vita di un token inizia quando viene emesso e termina quando scade o viene revocato. Durante questo periodo, i microservizi si basano sul token per stabilire l’identità dell’utente e applicare le decisioni di accesso.

Nei sistemi monolitici, erano le sessioni a gestire questo ruolo. Nei microservizi, i token colmano questa lacuna, ma sono più difficili da gestire a causa della loro natura distribuita.

![Ciclo di vita del Token](../../../assets/images/notes/token-based-security-standards/token/token-lifecycle.png)

<small> Fonte: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>