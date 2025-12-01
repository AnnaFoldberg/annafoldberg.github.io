---
title: "Individuelle Læringsmål: IT-sikkerhed (15 ECTS)"
categories: [it-security, microservices, kubernetes]
tags: [individual]
lang: da
locale: da
nav_order: 22
ref: learning-goals-it-security
---

##### Viden  
Jeg har:  
- Forståelse for east–west trafik og hvordan service meshes i Kubernetes håndterer mTLS-kryptering, service-identitet og sikkerhedspolitikker på tværs af services.  
- Kendskab til processen bag udstedelse og validering af tokens via OAuth 2.0 og OIDC, herunder brugen af Microsoft Entra som Identity Provider.  
- Viden om sikker håndtering af secrets, herunder principper for opbevaring i Kubernetes Secrets samt kendskab til ekstern secret storage.  
- Viden om logging og tracing som sikkerhedsværktøjer, herunder deres rolle i at sikre dataintegritet og opdage uautoriseret aktivitet.  
- Forståelse for shared responsibility i cloud-native miljøer, og hvordan ansvar for sikkerhed og drift fordeles mellem cloud-udbyder og udvikler.  
- Forståelse for, hvordan sikkerhedstiltag prioriteres ud fra systemets størrelse, trusselsmodel og arkitekturens kompleksitet.  

##### Færdigheder  
Jeg kan:  
- Opsætte en API-gateway løsning baseret på Kong til håndtering af indgående trafik med sikkerhedsfunktioner som TLS og rate limiting samt integration med OIDC-baseret autentifikation via oauth2-proxy.  
- Implementere zero-trust-principper ved at sikre token-validering i både oauth2-proxy og graph-gateway som led i det samlede adgangskontrolflow.  
- Opsætte og anvende Kubernetes Secrets via separate secret-manifester til brug i både Kubernetes-deployments og Helm-releases.  
- Opsætte TLS-terminering med selvsignerede certifikater via Ingress Controller og Ingress-ressource for at sikre krypteret trafik mellem klienten og indgangspunktet til systemet.  

##### Kompetencer  
Jeg kan:  
- Vurdere relevante sikkerhedstiltag i microservices- og kubernetesdrevede systemer.  
- Udarbejde sikre network policies til både Kubernetes-deployments og Helm-releases.  
- Designe en sikkerhedsarkitektur, der kombinerer krypteret trafik mellem klienten og systemets indgangspunkt med autentifikation og rate-limiting ved API-gatewayen, så uautoriserede kald og misbrugstrafik blokeres, før de når interne services.