---
title: "TeaApp: Simpel Microservices App med GraphQL, RabbitMQ, Kong og Docker"
categories: [mini, microservices, it-security]
tags: [graphql, rabbitmq, api-gateway, kong, docker, authentication, oauth2, architecture]
lang: da
locale: da
nav_order: 3
ref: project-tea-app-v1-da
---
TeaApp er en lille microservices-app, der simulerer bestilling og brygning af te.  

**Arkitekturoversigt**
- En **GraphQL-server** (`TeaApp.Api`), der håndterer queries, mutationer og subscriptions.  
- En **Brewer worker**, der modtager te-bestillinger og publicerer brewing/brewed events.  
- En **Notifier worker**, der abonnerer på brewed-events. Lige nu logger den kun, når brygningen er færdig, men sender ikke rigtige notifikationer.  
- En **konsolklient**, hvor man kan bestille te, se tilgængelige te-typer, hente en specifik ordre via orderId, se alle ordrer og følge live statusopdateringer via GraphQL subscriptions.  
- **Kong Gateway**, som fungerer som API-gateway.  
- **oauth2-proxy**, der validerer JWT-tokens ved kanten, før trafikken rammer API’et.  

**Nuværende funktionalitet**
- Se tilgængelige te-typer.  
- Afgiv en te-bestilling via en GraphQL-mutation.  
- Hent en specifik ordre via orderId.  
- Se en liste over alle ordrer.  
- Behandl bestillingen asynkront (Brewer simulerer brygning i trin).  
- Modtag live-opdateringer (brewing / brewed) gennem GraphQL subscriptions.  
- Håndhæv authentication (via oauth2-proxy) og rate limiting på API-gatewayen.  

**Teknologier**
- .NET 9 / ASP.NET Core  
- GraphQL (HotChocolate)  
- RabbitMQ (AMQP)  
- Kong (API Gateway, rate limiting)  
- oauth2-proxy + Microsoft Entra ID (OIDC/JWT auth)  
- Docker + Docker Compose  

[TeaApp Repository](https://github.com/annafoldberg/tea-app)