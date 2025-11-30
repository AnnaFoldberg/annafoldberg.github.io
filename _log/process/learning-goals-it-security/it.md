---
title: "Obiettivi di Apprendimento Individuali: Sicurezza IT (15 ECTS)"
categories: [it-security, microservices, kubernetes]
tags: [individual]
lang: it
locale: it
nav_order: 22
ref: learning-goals-it-security
---

**Conoscenze**

Ho:  
- Una comprensione del traffico east–west in Kubernetes e di come i service mesh gestiscono mTLS, identità dei servizi e policy di sicurezza tra i servizi.  
- Conoscenza del processo di emissione e validazione dei token in OAuth 2.0 e OIDC, incluso l’utilizzo di Microsoft Entra come Identity Provider.  
- Conoscenza della gestione sicura dei secrets, compresi i principi per conservarli in Kubernetes Secrets e le opzioni per un secret storage esterno.  
- Conoscenza del logging e del tracing come strumenti di sicurezza, inclusa la loro importanza per l’integrità dei dati e il rilevamento di attività non autorizzate.  
- Comprensione del modello di responsabilità condivisa negli ambienti cloud-native e della divisione delle responsabilità tra cloud provider e sviluppatore.  
- Conoscenza della valutazione dei rischi e dei metodi per la prioritizzazione delle misure di sicurezza.

**Abilità**

So:  
- Configurare una soluzione API gateway basata su Kong per gestire il traffico in ingresso con funzionalità di sicurezza come TLS, rate limiting e autenticazione basata su OIDC tramite oauth2-proxy.  
- Implementare principi zero-trust garantendo la validazione dei token sia in oauth2-proxy sia in graph-gateway come parte del flusso di controllo degli accessi.  
- Configurare comunicazione TLS con certificati auto-firmati tra client e load balancer Kubernetes per proteggere il traffico al di fuori del cluster.

**Competenze**

Sono in grado di:  
- Progettare network policy sicure per i deployments Kubernetes e per applicazioni basate su Helm.  
- Costruire un’architettura di sicurezza coerente che consente l’accesso solo ai client autorizzati tramite autenticazione JWT/OIDC e che rifiuta le richieste non autorizzate all’API gateway.  
- Valutare e prioritizzare le misure di sicurezza rilevanti in sistemi basati su microservizi e Kubernetes.