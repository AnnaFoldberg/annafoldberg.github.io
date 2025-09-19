---
title: "Security in Microservices"
categories: [microservices, it-security]
tags: [authentication, encryption, observability]
lang: en
locale: en
nav_order: 2
ref: note-microservices-security
---
Security in a microservices architecture requires measures at both the service and infrastructure level. Each service is independently deployed, which means that authentication, communication, data protection, and observability must all be handled consistently across the system.

##### Authentication & Authorization
- Strong identity management frameworks (OAuth2, OpenID Connect, JWT)  
- **Zero trust principle**: Every service call must be authenticated  
- **Role-based access control (RBAC)** at both user and service level  

##### Service-to-Service Communication
- **Mutual TLS (mTLS)** to secure traffic and verify service identity  
- **Service meshes** (e.g. Istio, Linkerd) for mTLS and policy enforcement  

##### API Security
- Protection of REST/gRPC APIs with **rate limiting**, **input/schema validation**, and API gateways  
- Defence against common attacks (injection, replay, broken object-level authorization – OWASP API Top 10)  

##### Data Protection
- Encryption of data at rest (databases, message queues)  
- Masking or tokenization of sensitive information (PII, payment data)  
- Dedicated **secrets management** solution (Azure Key Vault, Kubernetes Secrets)  

##### Observability & Monitoring
- Centralization of logging with correlation IDs across services  
- Monitoring for anomalies (e.g. failed authentication attempts, traffic spikes)  
- Detection of potential **lateral movement** attempts inside the network  