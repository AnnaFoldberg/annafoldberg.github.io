---
title: "Trackunit: Security Responsibility in the System"
categories: [trackunit, it-security]
tags: [oauth2, authorization, jwt, final]
lang: en
locale: en
nav_order: 48
ref: security-responsibility
---

##### Division of responsibility

In our setup, security responsibilities are split between **oauth2-proxy** and **graph-gateway**, so authentication and authorization are handled in different layers of the system.

**oauth2-proxy** acts as the edge component and performs the initial validation of JWTs.  
It ensures that the token was issued by Microsoft Entra ID for our application and rejects invalid tokens *before* traffic enters the cluster.

**graph-gateway** handles authorization at the API level.  
It uses the `RequireApiScope` policy, which verifies that the token contains the scope required to access the GraphQL API.

##### Benefits

This separation provides **defense in depth** and enables the introduction of different user groups in the future, where specific queries, mutations, or subscriptions can be restricted to tokens with particular scopes.  

This allows the API to evolve with role-specific or client-specific functionality — without modifying the configuration of either the gateway or **oauth2-proxy**.