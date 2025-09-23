---
title: "TeaApp: Semplice Applicazione a Microservizi con GraphQL, RabbitMQ, Kong e Docker"
categories: [mini, microservices, it-security]
tags: [graphql, rabbitmq, api-gateway, kong, docker, autenticazione, oauth2, architettura]
lang: it
locale: it
nav_order: 3
ref: project-tea-app-v1
---
TeaApp è una mini applicazione a microservizi che simula l’ordinazione e la preparazione del tè.  

**Panoramica dell'architettura**
- Un **server GraphQL** (`TeaApp.Api`) che accetta query, mutazioni e sottoscrizioni.  
- Un **Brewer worker** che consuma ordini di tè e pubblica eventi di preparazione/completamento.  
- Un **Notifier worker** che si sottoscrive agli eventi di completamento. Attualmente registra solo quando la preparazione è terminata, ma non invia notifiche reali.  
- Un **client console** per effettuare ordini, visualizzare i tipi di tè, recuperare un ordine specifico tramite orderId, elencare tutti gli ordini e seguire aggiornamenti live tramite sottoscrizioni GraphQL.  
- **Kong Gateway** che funge da API gateway.  
- **oauth2-proxy** che valida i JWT al perimetro prima che il traffico raggiunga l’API.  

**Funzionalità attuali**
- Visualizzare i tipi di tè disponibili.  
- Effettuare un ordine di tè tramite mutazione GraphQL.  
- Recuperare un ordine specifico tramite orderId.  
- Elencare tutti gli ordini.  
- Elaborare l’ordine in modo asincrono (il Brewer simula i passaggi della preparazione).  
- Ricevere aggiornamenti live (preparazione / completato) tramite sottoscrizioni GraphQL.  
- Applicare autenticazione (tramite oauth2-proxy) e rate limiting a livello di API gateway.  

**Tecnologie**
- **.NET 9 / ASP.NET Core**  
- **GraphQL (HotChocolate)**  
- **RabbitMQ (AMQP)**  
- **Kong (API Gateway, rate limiting)**  
- **oauth2-proxy + Microsoft Entra ID (OIDC/JWT auth)**  
- **Docker + Docker Compose**  

[Repository TeaApp](https://github.com/annafoldberg/tea-app)