---
title: "Securing East–West Traffic"
categories: [microservices, it-security]
tags: [api-security, jwt, tokens, access-control]
lang: en
locale: en
nav_order: 27
ref: note-east-west-traffic
---
Microservices follow the single responsibility pattern, which makes secure communication between them critical. East–west traffic refers to service-to-service calls inside the system, raising questions about how user identity is shared and where access policies are enforced.

A common anti-pattern is to use one access token with a shared scope across services. This creates tight coupling, weakens least-privilege principles, and risks exposing sensitive services directly.

One strategy is for a service to request its own access token using the client credentials grant before calling another service. This separates scopes, but the downstream service loses awareness of the end user and must trust the caller. If compromised, that caller could act on behalf of any user.

A stronger approach is to pass user identity along with the access token, typically through a JWT containing claims about the end user. By verifying the JWT signature, the downstream service can enforce its own access control decisions. To protect user data, clients often receive only a reference token; at the API gateway, this is exchanged for a structured token containing claims that is passed internally between services.

These techniques allow microservices to establish user context without shared session state. Careful design is needed to prevent scopes or claims from coupling services too tightly while still enabling secure collaboration.

<small> Source: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>