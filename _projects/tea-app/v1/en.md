---
title: "TeaApp: Simple Microservices App with GraphQL, RabbitMQ, Kong, and Docker"
categories: [mini, microservices, it-security]
tags: [graphql, rabbitmq, api-gateway, kong, docker, authentication, oauth2, architecture]
lang: en
locale: en
nav_order: 3
ref: project-tea-app-v1
---
TeaApp is a mini microservices app that simulates ordering and brewing tea.  

**Architecture Overview**
- A **GraphQL server** (`TeaApp.Api`) that accepts queries, mutations, and subscriptions.  
- A **Brewer worker** that consumes tea orders and publishes brewing/brewed events.  
- A **Notifier worker** that subscribes to brewed events. Currently, it only logs when brewing is finished but does not send real notifications.  
- A **console client** to place orders, list teas, view a specific order by orderId, list all orders, and follow live brewing updates via GraphQL subscriptions.  
- **Kong Gateway** acting as the API gateway.  
- **oauth2-proxy** validating JWTs at the edge before traffic reaches the API.  

**Current Capabilities**
- View available teas.  
- Place an order for tea via GraphQL mutation.  
- Fetch a specific order by orderId.  
- List all orders.  
- Process the order asynchronously (Brewer worker simulates brewing steps).  
- Receive live updates (brewing / brewed) through GraphQL subscriptions.  
- Enforce authentication (via oauth2-proxy) and rate limiting at the API gateway level.  

**Technologies**
- .NET 9 / ASP.NET Core  
- GraphQL (HotChocolate)  
- RabbitMQ (AMQP)  
- Kong (API Gateway, rate limiting)  
- oauth2-proxy + Microsoft Entra ID (OIDC/JWT auth)  
- Docker + Docker Compose  

[TeaApp Repository](https://github.com/annafoldberg/tea-app)