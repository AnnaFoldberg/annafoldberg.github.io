---
title: "Sicurezza nei microservizi"
categories: [microservices, it-security]
tags: [overview]
lang: it
locale: it
ref: note-microservices-security
---
La sicurezza in un’architettura a microservizi richiede misure sia a livello di servizio sia a livello di infrastruttura. Ogni servizio è distribuito in modo indipendente, il che significa che autenticazione, autorizzazione, protezione dei dati e observability devono essere gestite in modo coerente in tutto il sistema.

##### Autenticazione e autorizzazione
- Solidi identity management frameworks (OAuth2, OpenID Connect, JWT)  
- **Principio zero trust**: Ogni chiamata di servizio deve essere autenticata  
- **Controllo degli accessi basato sui ruoli (RBAC)** sia a livello utente che di servizio  

##### Comunicazione tra servizi
- **Mutual TLS (mTLS)** per proteggere il traffico e verificare l’identità dei servizi  
- **Service mesh** (es. Istio, Linkerd) per mTLS e policy enforcement  

##### Sicurezza delle API
- Protezione delle API REST/gRPC con **rate limiting**, **input/schema validation** e API gateways  
- Difesa contro attacchi comuni (injection, replay, broken object-level authorization – OWASP API Top 10)  

##### Protezione dei dati
- Crittografia dei dati a riposo (database, message queues)  
- Mascheramento o tokenizzazione dei dati sensibili (PII, dati di pagamento)  
- Soluzione dedicata di **secrets management** (Azure Key Vault, Kubernetes Secrets)  

##### Observability e monitoraggio
- Logging centralizzato con correlation IDs tra i servizi  
- Monitoraggio delle anomalie (es. tentativi di autenticazione falliti, traffic spikes)  
- Rilevamento di potenziali tentativi di **lateral movement** all’interno della rete  