---
title: "Sikkerhed i Microservices"
categories: [microservices, it-security]
tags: [authentication, encryption, observability]
lang: da
locale: da
nav_order: 2
ref: note-microservices-security
---
Sikkerhed i en microservices-arkitektur kræver tiltag både på service- og infrastrukturniveau. Hver service udrulles uafhængigt, hvilket betyder, at autentificering, autorisation, databeskyttelse og observality skal håndteres konsekvent på tværs af systemet.

##### Autentificering og autorisation
- Stærke identity management-frameworks (OAuth2, OpenID Connect, JWT)  
- **Zero trust-princippet**: Hvert servicekald skal autentificeres  
- **Rollebaseret adgangskontrol (RBAC)** på både bruger- og serviceniveau  

##### Service-til-service kommunikation
- **Mutual TLS (mTLS)** for at sikre trafik og verificere service-identitet  
- **Service meshes** (f.eks. Istio, Linkerd) til mTLS og policy enforcement  

##### API-sikkerhed
- Beskyttelse af REST/gRPC APIs med **rate limiting**, **input/schema validation** og API gateways  
- Forsvar mod almindelige angreb (injection, replay, broken object-level authorization – OWASP API Top 10)  

##### Databeskyttelse
- Kryptering af data i hvile (databaser, message queues)  
- Maskering eller tokenisering af følsomme data (PII, betalingsdata)  
- Dedikeret **secrets management**-løsning (Azure Key Vault, Kubernetes Secrets)  

##### Observability og monitorering
- Centraliseret logning med correlation IDs på tværs af services  
- Overvågning for anomalier (f.eks. fejlede autentificeringsforsøg, traffic spikes)  
- Detektion af potentiel **lateral movement** i netværket  