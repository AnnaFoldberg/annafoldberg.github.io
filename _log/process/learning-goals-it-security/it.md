---
title: "Obiettivi di Apprendimento Individuali: Sicurezza IT (15 ECTS)"
categories: [it-security, microservices, kubernetes]
tags: [individual]
lang: it
locale: it
nav_order: 22
ref: learning-goals-it-security
---

##### Conoscenza  
Ho:  
- Comprensione del traffico east–west e di come i service mesh in Kubernetes gestiscono la crittografia mTLS, l’identità dei servizi e le politiche di sicurezza tra i servizi.  
- Conoscenza del processo di emissione e validazione dei token tramite OAuth 2.0 e OIDC, incluso l’uso di Microsoft Entra come Identity Provider.  
- Conoscenza della gestione sicura dei secrets, inclusi i principi di archiviazione in Kubernetes Secrets e la familiarità con sistemi di secret storage esterni.  
- Conoscenza di logging e tracing come strumenti di sicurezza, incluso il loro ruolo nel garantire l’integrità dei dati e rilevare attività non autorizzate.  
- Comprensione del modello di responsabilità condivisa negli ambienti cloud-native e della divisione delle responsabilità tra cloud provider e sviluppatore.  
- Comprensione di come le misure di sicurezza vengono priorizzate in base alla dimensione del sistema, al modello di minaccia e alla complessità architetturale.  

##### Abilità  
Posso:  
- Configurare una soluzione di API gateway basata su Kong per gestire il traffico in ingresso con funzionalità di sicurezza come TLS e rate limiting, oltre all’integrazione con l’autenticazione basata su OIDC tramite oauth2-proxy.  
- Implementare principi zero-trust garantendo la validazione dei token sia in oauth2-proxy che in graph-gateway come parte del flusso complessivo di controllo degli accessi.  
- Configurare e utilizzare Kubernetes Secrets tramite manifest dedicati per l’uso in Kubernetes deployments e Helm releases.  
- Configurare la terminazione TLS con certificati self-signed tramite un Ingress Controller e una risorsa Ingress per garantire traffico crittografato tra il client e il punto di ingresso del sistema.  

##### Competenze  
Posso:  
- Valutare le misure di sicurezza rilevanti nei sistemi basati su microservizi e Kubernetes.  
- Sviluppare network policy sicure per Kubernetes deployments e Helm releases.  
- Progettare un’architettura di sicurezza che combini traffico crittografato tra il client e il punto di ingresso del sistema con autenticazione e rate limiting all’API gateway, bloccando traffico non autorizzato o abusivo prima che raggiunga i servizi interni.