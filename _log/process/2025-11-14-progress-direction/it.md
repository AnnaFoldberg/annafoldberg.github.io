---
title: "2025-11-14: Progressi e Direzione"
categories: [microservices, kubernetes, it-security]
tags: [individual, process, feedback, feed-forward]
lang: it
locale: it
nav_order: 17
ref: log-2025-11-14-progress-direction
---

##### Feedback

Ho ora spostato l’ambiente Trackunit da docker-compose a Kubernetes.

Ho creato deployments, services, secrets, namespaces, network policies e file values per Helm da utilizzare nei relativi chart. Tutti i microservizi vengono eseguiti come pod gestiti dai deployments ed esposti internamente tramite servizi ClusterIP, mentre solo Kong è esposto esternamente tramite un servizio LoadBalancer. I componenti infrastrutturali come **kong**, **rabbitmq** e **oauth2-proxy** vengono eseguiti come risorse Kubernetes tramite Helm. In questo modo l’intero setup è diventato più flessibile, scalabile e robusto.

Utilizzo `kind` per eseguire il mio cluster Kubernetes locale, poiché offre un ambiente realistico e rende possibile testare il servizio LoadBalancer di Kong tramite `cloud-provider-kind`. Kong è l’unico servizio esposto al di fuori del cluster e funge da punto di ingresso al sistema.

Per gestire e monitorare il cluster utilizzo `k9s`, che offre una panoramica rapida ed efficiente di deployments, pod, log ed eventi.

##### Feed-forward

Il mio prossimo obiettivo sarà introdurre il versionamento delle container images, in modo da gestire meglio i rilasci e consentire a Kubernetes di eseguire rolling update puliti quando vengono distribuite nuove versioni delle immagini.

Successivamente implementerò la terminazione TLS tra il client e **kong-proxy**, in modo che tutto il traffico al di fuori del cluster sia crittografato durante il transito.

Scelgo di non introdurre mTLS, né richiedendo un certificato al client né internamente tra i pod in Kubernetes tramite un service mesh, a causa delle dimensioni del progetto e perché il traffico interno è già protetto esternamente dall’API gateway. Anche in un ambiente di produzione, un attaccante dovrebbe superare sia l’API gateway sia l’autorizzazione in graph-gateway prima di poter accedere alla comunicazione interna tra pod.  

Quando i restanti microservizi saranno implementati, **svc-analysis-orchestrator** verrà ampliato per gestire il flusso di analisi dopo l’upload. Verrà anche introdotto un messaging bridge, che fungerà da adattatore tra RabbitMQ e Kafka, poiché i miei microservizi esistenti utilizzano RabbitMQ mentre i nuovi servizi utilizzano Kafka. Allo stesso tempo, **graph-gateway** verrà esteso con endpoint GraphQL che inoltrano richieste a tu-ingestion-service tramite i suoi endpoint REST e gestiscono le risposte restituite.