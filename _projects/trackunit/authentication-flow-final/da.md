---
title: "Trackunit Prototype: Authentication Flow"
categories: [trackunit, microservices, it-security]
tags: [authentication, api-gateway, kong, oauth2, prototype]
lang: da
locale: da
nav_order: 20
ref: authentication-flow-final
---
>**Note:** Autentificeringsflowet er identisk med det, der blev brugt i prototypen. Det gengives her for tydelighedens skyld og for at sikre, at dokumentationen er selvstændigt forståelig.  
1. **Client** autentificerer sig via **MSAL** (device code) og opnår et **access token** til API’et:  
   - **Scope anmodet:** `api://{API_CLIENT_ID}/analysis` → tokenets `scp` indeholder `analysis`.  
   - **Token audience:** `api://{API_CLIENT_ID}` eller `{API_CLIENT_ID}`
2. **Kong** proxy'er `/graphql` til **oauth2-proxy**.  
3. **oauth2-proxy** validerer **Bearer token** mod **Microsoft Entra ID**:  
   - Verificerer *issuer* og *audience*  
   - Hvis tokenet er gyldigt, videresendes requesten til upstream-tjenesten  
4. **GraphGateway** re-validerer JWT’et og håndhæver GraphQL-autoriseringspolitikken `RequireApiScope` på beskyttede felter.