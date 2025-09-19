---
title: "Manutenzione e Protezione dei Token"
categories: [microservices, it-security]
tags: [tokens, oauth2]
lang: it
locale: it
nav_order: 25
ref: note-token-maintenance
---
Nel migliore dei casi, un token è valido fino alla sua data di scadenza, dopo la quale non può più essere utilizzato per accedere a un microservizio. Gli access token sono tipicamente di breve durata, definiti da un claim *expires_in* o equivalente. Durate brevi riducono il rischio se un token viene compromesso. Quando è necessario un accesso più lungo, può essere emesso un refresh token per ottenere nuovi access token senza l’intervento dell’utente.

I refresh token comportano rischi propri, poiché assomigliano a password se rubati. Per mitigare questo rischio, possono essere ruotati, emettendo un nuovo refresh token ogni volta che uno viene utilizzato.

Nel peggiore dei casi, i token compromessi devono essere revocati. Questo richiede che i token vengano memorizzati e spesso tutti i token concessi a un utente devono essere invalidati. Se i token non sono persistiti, la revoca è possibile solo attendendo la scadenza. OAuth definisce una specifica di revoca dei token che fornisce un endpoint per revocare esplicitamente i token.

**Best Practices**

- Utilizzare sempre TLS per il trasporto  
- Proteggere i token come credenziali, incluso client ID e secret  
- Impostare date di scadenza, mantenerle brevi e utilizzare i refresh token se necessario  
- Non inserire informazioni sensibili nel payload del token  

<small> Fonte: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>