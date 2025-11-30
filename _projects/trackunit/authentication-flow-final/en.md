---
title: "Trackunit: Authentication Flow"
categories: [trackunit, microservices, it-security]
tags: [authentication, api-gateway, kong, oauth2, final]
lang: en
locale: en
nav_order: 20
ref: authentication-flow-final
---
>**Note:** The authentication flow is identical to the one used in the prototype. It is repeated here for clarity and to ensure that the documentation is self-contained.  

1. **Client** authenticates with **MSAL** (device code) and acquires an **access token** for the API:
   - **Scope requested:** `api://{API_CLIENT_ID}/analysis` → token `scp` includes `analysis`.
   - **Token audience:** `api://{API_CLIENT_ID}` or `{API_CLIENT_ID}`
2. **Kong** proxies `/graphql` to **oauth2-proxy**.
3. **oauth2-proxy** validates the Bearer token against Microsoft Entra ID:
   - Verifies issuer and audience
   - If valid, forwards the request upstream
4. **GraphGateway** re-validates the JWT and enforces the GraphQL authorization policy `RequireApiScope` on protected fields.