---
title: "Obiettivi di Apprendimento Individuali: Microservizi & Kubernetes (15 ECTS)"
categories: [microservices, kubernetes]
tags: [individual]
lang: it
locale: it
nav_order: 21
ref: learning-goals-microservices-kubernetes
---

##### Conoscenza  
Ho:  
- Comprensione dei diversi tipi di servizi in un sistema a microservizi, inclusi microservizi basati sul dominio, sui processi aziendali e sulle transazioni atomiche.  
- Conoscenza dei principali pattern di integrazione come API Gateway Pattern, Aggregator Pattern ed Edge Pattern.  
- Conoscenza dei pattern operativi, inclusi Log Aggregation Pattern, Metrics Aggregation Pattern, Tracing Pattern, configurazione esterna e service discovery.  
- Comprensione delle differenze architetturali tra microservizi e monoliti, incluso come la scelta architetturale influisce su scalabilità, complessità, isolamento dei guasti e adeguatezza complessiva rispetto al progetto specifico.  
- Conoscenza dei meccanismi di gestione degli errori e diagnostica in Kubernetes, inclusi `kubectl logs` e `kubectl describe`.  
- Comprensione dei meccanismi di health, readiness e liveness nei microservizi e in Kubernetes.  
- Conoscenza dell’architettura di Kubernetes, inclusi i componenti del control plane (etcd, kube-apiserver, scheduler, controller-manager) e i componenti dei nodi (kubelet, kube-proxy, container runtime).  
- Comprensione delle differenze tra i principali tipi di Service in Kubernetes, inclusi ClusterIP, NodePort e LoadBalancer.  
- Conoscenza dei vantaggi degli Helm chart rispetto ai tradizionali manifest Kubernetes, inclusa l’utilizzo dei values file per una configurazione coerente e scalabile.

##### Abilità  
Posso:  
- Valutare e selezionare i tipi di servizio, i pattern di integrazione e i pattern operativi più appropriati per risolvere problemi specifici.  
- Configurare e utilizzare RabbitMQ come message broker tramite il protocollo AMQP per comunicazione basata su eventi e comandi.  
- Utilizzare e adattare Helm chart esistenti tramite values file.  
- Configurare e amministrare cluster Kubernetes locali con kind.  
- Configurare strategie di rolling update in Kubernetes utilizzando le impostazioni maxSurge e maxUnavailable per supportare deployment senza downtime.  
- Utilizzare immagini container versionate per ottenere aggiornamenti controllati e rollback efficienti.  
- Utilizzare `kubectl logs` e `kubectl describe` per il troubleshooting di pod e deployment in Kubernetes.

##### Competenze  
Posso:  
- Progettare e pianificare l’architettura di un sistema basato su microservizi, con attenzione a servizi poco accoppiati, comunicazione asincrona e scalabilità.  
- Amministrare e mantenere ambienti Kubernetes basati su kind, lavorando con deployment, servizi, secret, namespace e network policy.