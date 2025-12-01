---
title: "Trackunit: Responsabilità della sicurezza nel sistema"
categories: [trackunit, it-security]
tags: [oauth2, authorization, jwt, final]
lang: it
locale: it
nav_order: 48
ref: security-responsibility
---

##### Suddivisione delle responsabilità

Nel nostro setup, la responsabilità della sicurezza è suddivisa tra **oauth2-proxy** e **graph-gateway**, così che autenticazione e autorizzazione vengano gestite in livelli distinti del sistema.

**oauth2-proxy** funge da componente edge ed esegue la validazione iniziale dei JWT.  
Garantisce che il token sia stato emesso da Microsoft Entra ID per la nostra applicazione e rifiuta i token non validi *prima* che il traffico entri nel cluster.

**graph-gateway** gestisce l’autorizzazione a livello di API.  
Qui viene utilizzata la policy `RequireApiScope`, che verifica che il token contenga lo scope necessario per accedere all’API GraphQL.

##### Vantaggi

Questa separazione fornisce **defense in depth** e permette di introdurre in futuro gruppi di utenti diversi, in cui specifiche query, mutation o subscription sono disponibili solo per token che possiedono determinati scope.  

In questo modo l’API può essere estesa con funzionalità specifiche per ruoli o client, senza modificare la configurazione né della gateway né di **oauth2-proxy**.