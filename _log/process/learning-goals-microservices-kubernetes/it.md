---
title: "Obiettivi di Apprendimento Individuali: Microservizi & Kubernetes (15 ECTS)"
categories: [microservices, kubernetes]
tags: [individual]
lang: it
locale: it
nav_order: 21
ref: learning-goals-microservices-kubernetes
---

**Conoscenze**

Ho:  
- Comprensione dei diversi tipi di servizi in un sistema a microservizi, inclusi microservizi basati sul dominio, sul processo aziendale e sulle transazioni atomiche.  
- Conoscenza dei principali pattern di integrazione come API Gateway Pattern, Aggregator Pattern ed Edge Pattern.  
- Conoscenza dei pattern operativi, tra cui Log Aggregation Pattern, Metrics Aggregation, Tracing Pattern, configurazione esterna e service discovery.  
- Comprensione delle differenze architetturali tra microservizi e monoliti, inclusi gli impatti sulle prestazioni, sulla scalabilità, sulla complessità e sull’isolamento dei guasti in relazione al progetto specifico.  
- Conoscenza dei meccanismi di diagnostica e gestione degli errori in Kubernetes, come `kubectl logs` e `kubectl describe`.  
- Comprensione dei meccanismi di health, readiness e liveness nei microservizi e in Kubernetes.  
- Conoscenza dell’architettura di Kubernetes, inclusi i componenti del control-plane (etcd, kube-apiserver, scheduler, controller-manager) e i componenti dei worker node (kubelet, kube-proxy, container runtime).  
- Comprensione delle differenze tra i vari tipi di servizi Kubernetes (ClusterIP, NodePort, LoadBalancer).  
- Conoscenza dei vantaggi dei Helm chart rispetto al deployment manuale, tra cui templating, riuso delle configurazioni e maggiore coerenza negli ambienti Kubernetes scalabili.

**Abilità**

Sono in grado di:  
- Valutare e selezionare i tipi di servizi e i pattern di integrazione più adeguati per risolvere problemi architetturali concreti.  
- Configurare e utilizzare RabbitMQ come message broker tramite il protocollo AMQP per comunicazione basata su eventi e comandi.  
- Utilizzare e adattare Helm chart, inclusi i values file e i secret, per il deployment in cluster Kubernetes locali.  
- Configurare e gestire cluster Kubernetes locali con Kind.  
- Configurare strategie di rolling update in Kubernetes utilizzando container-image versionate e impostazioni come `maxSurge` e `maxUnavailable` per ottenere zero-downtime deployments.  
- Utilizzare `kubectl logs` e `kubectl describe` per risolvere problemi relativi a pod e deployment.

**Competenze**

Sono capace di:  
- Progettare e strutturare l’architettura di un sistema basato su microservizi, con attenzione al loose coupling, alla comunicazione asincrona e alla scalabilità.  
- Amministrare e mantenere ambienti Kubernetes basati su Kind, lavorando con deployment, namespace, network policy, servizi e secret.