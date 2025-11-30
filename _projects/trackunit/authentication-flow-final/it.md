---
title: "Trackunit: Flusso di autenticazione"
categories: [trackunit, microservices, it-security]
tags: [authentication, api-gateway, kong, oauth2, final]
lang: it
locale: it
nav_order: 20
ref: authentication-flow-final
---
>**Nota:** Il flusso di autenticazione è identico a quello utilizzato nel prototipo. È riportato qui per maggiore chiarezza e per garantire che la documentazione sia autosufficiente.  

1. Il **client** si autentica tramite **MSAL** (device code) e ottiene un **access token** per l’API:  
   - **Scope richiesto:** `api://{API_CLIENT_ID}/analysis` → il campo `scp` del token include `analysis`.  
   - **Audience del token:** `api://{API_CLIENT_ID}` oppure `{API_CLIENT_ID}`
2. **Kong** effettua il proxy di `/graphql` verso **oauth2-proxy**.  
3. **oauth2-proxy** valida il **Bearer token** contro **Microsoft Entra ID**:  
   - Verifica *issuer* e *audience*  
   - Se valido, inoltra la richiesta al servizio upstream  
4. **GraphGateway** riesegue la validazione del JWT e applica la policy di autorizzazione GraphQL `RequireApiScope` sui campi protetti.