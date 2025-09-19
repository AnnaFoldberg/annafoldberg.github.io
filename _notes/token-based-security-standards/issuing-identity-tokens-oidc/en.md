---
title: "Issuing Identity Tokens with OIDC"
categories: [microservices, it-security]
tags: [oidc, tokens, authentication]
lang: en
locale: en
nav_order: 24
ref: note-issuing-identity-tokens-oidc
---
OpenID Connect is a thin identity layer that sits on top of OAuth. The standard describes how capabilities like authentication and user profile information are delivered using an authentication request, an ID token and a user info endpoint.

OpenID Connect:

- Establishes a central point to identify (authenticate) end users  
- Decouples authentication from client applications  

The identity token, which is in JWT format, contains a standard set of claims that provide information regarding the authentication event, the token issuer, the end-user and the token expiration.

Since the ID token is a JWT, it contains a cryptographic signature that protects the token from being tampered with.

Identity tokens should only be used by clients and should not be used for API access. To establish user identity within a microservice, the access token can be passed to the identity provider's user info endpoint to receive claims with information about the end-user. This allows the microservice to get information about the end-user.

![OIDC Identity Token](../../../assets/images/notes/token-based-security-standards/oidc/oidc-identity-token.png)

<small> Source: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>