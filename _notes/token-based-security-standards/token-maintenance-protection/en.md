---
title: "Token Maintenance and Protection"
categories: [microservices, it-security]
tags: [tokens, oauth2]
lang: en
locale: en
nav_order: 25
ref: note-token-maintenance
---
In the best case scenario, a token is valid until its expiration date, after which it can no longer be used to access a microservice. Access tokens are typically short-lived, defined by an *expires_in* claim or equivalent. Short lifetimes reduce risk if a token is compromised. When longer access is required, a refresh token can be issued to obtain new access tokens without user involvement.

Refresh tokens carry their own risks, as they resemble passwords if stolen. To mitigate this, they can be rotated, issuing a new refresh token each time one is used.

In the worst case scenario, compromised tokens must be revoked. This requires tokens to be stored, and often all tokens granted to a user must be invalidated. If tokens are not persisted, revocation is only possible by waiting for expiration. OAuth defines a token revocation specification that provides an endpoint to explicitly revoke tokens.

**Best Practices**

- Always use TLS for transport  
- Protect tokens like credentials, including client ID and secret  
- Set expiration dates, keep them short, and use refresh tokens if needed  
- Do not embed sensitive information in token payloads  

<small> Source: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>