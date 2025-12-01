---
title: "Trackunit: Sikkerhedsansvar i systemet"
categories: [trackunit, it-security]
tags: [oauth2, authorization, jwt, final]
lang: da
locale: da
nav_order: 48
ref: security-responsibility
---

##### Fordeling af ansvar

I vores setup er ansvaret for sikkerhed fordelt mellem **oauth2-proxy** og **graph-gateway**, så autentificering og autorisation håndteres hvert sit sted i systemet.

**oauth2-proxy** fungerer som edge-komponent og står for den indledende validering af JWT’er.  
Den sikrer, at tokenet er udstedt af Microsoft Entra ID til vores applikation og afviser ugyldige tokens, *før* trafikken når ind i clusteret.

**graph-gateway** håndterer autorisation på API-niveau.  
Her anvendes politikken `RequireApiScope`, som kontrollerer, at tokenet indeholder det scope, der kræves for at kalde GraphQL-API’et.

##### Fordele

Denne opdeling giver **defense in depth** og gør det muligt at introducere forskellige brugergrupper senere, hvor specifikke queries, mutationer eller subscriptions kun er tilgængelige for tokens med bestemte scopes.  

På den måde kan API’et udvides med funktioner målrettet specifikke roller eller klienter, uden at ændre konfigurationen af hverken gatewayen eller **oauth2-proxy**.