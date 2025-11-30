---
title: "Individuelle Læringsmål: IT-sikkerhed (15 ECTS)"
categories: [it-security, microservices, kubernetes]
tags: [individual]
lang: da
locale: da
nav_order: 22
ref: learning-goals-it-security
---

**Viden**

Jeg har:  
- Forståelse for east–west trafik i Kubernetes og hvordan service meshes håndterer mTLS, service-identitet og sikkerhedspolitikker mellem services.  
- Kendskab til processen bag udstedelse og validering af tokens i OAuth 2.0 og OIDC, herunder brugen af Microsoft Entra som Identity Provider.  
- Viden om sikker håndtering af secrets, herunder principper for opbevaring i Kubernetes Secrets og muligheder for ekstern secret storage.  
- Viden om logging og tracing som sikkerhedsværktøjer, herunder deres betydning for dataintegritet og detektion af uautoriseret aktivitet.  
- Forståelse for shared responsibility i cloud-native miljøer og fordelingen af ansvar mellem cloud-udbyder og udvikler.  
- Kendskab til risikovurdering og metoder til prioritering af sikkerhedstiltag.

**Færdigheder**

Jeg kan:  
- Opsætte en API-gateway-løsning baseret på Kong til håndtering af indgående trafik, med sikkerhedsfunktioner som TLS, rate limiting og OIDC-baseret autentifikation via oauth2-proxy.  
- Implementere zero-trust-principper ved at sikre token-validering i både oauth2-proxy og graph-gateway som del af den samlede adgangskontrol.  
- Opsætte TLS-kommunikation med selvsignerede certifikater mellem klienter og Kubernetes-load balancere for at sikre krypteret trafik uden for clusteret.

**Kompetencer**

Jeg kan:  
- Udarbejde sikre network policies til både Kubernetes-deployments og Helm-baserede applikationer.  
- Opbygge en sammenhængende sikkerhedsarkitektur, der kun tillader autoriserede klienter adgang via JWT/OIDC-baseret autentifikation, og som afviser uautoriserede kald ved API-gatewayen.  
- Evaluere og prioritere relevante sikkerhedstiltag i microservices- og Kubernetes-baserede systemer.