---
title: "Logging e Tracing"
categories: [microservices, it-security]
tags: [logging, tracing, observability]
lang: it
locale: it
nav_order: 28
ref: note-logging-tracing
---
Il logging è uno degli strumenti più importanti per l’osservabilità in un sistema distribuito. Supporta il rilevamento di comportamenti sospetti, l’indagine sugli incidenti, l’audit delle operazioni sensibili e il troubleshooting dei guasti. Per essere efficace, i log devono seguire una struttura coerente concordata da tutti i team di microservizi. Framework standard e configurazioni condivise aiutano a far rispettare modelli uniformi. I log dovrebbero includere sia attività riuscite che fallite, come errori di autorizzazione o endpoint non validi, poiché questi possono indicare tentativi di scansione o di attacco.

Poiché i microservizi sono distribuiti, i log di tutte le istanze devono essere aggregati in un host centrale. Questo crea un quadro completo dell’attività del sistema e consente il monitoraggio automatico per rilevare anomalie come handshake mTLS falliti, token non validi o schemi di traffico insoliti. Gli avvisi possono quindi essere inviati tramite sistemi di incident response.

![Logging e Monitoraggio](../../../assets/images/notes/security-between-microservices/logging-tracing/logging-monitoring.png)

Per catturare il flusso completo di una richiesta tra i servizi, i log devono anche essere correlati. Questo si ottiene tramite il tracing. A ogni richiesta viene assegnato un identificatore di traccia univoco, che è incluso in tutti i log e nell’output temporale relativi a quella richiesta. Ogni servizio che riceve la richiesta continua a utilizzare lo stesso identificatore e lo trasmette a valle. Questo rende possibile ricostruire l’intero percorso di una richiesta attraverso il sistema e analizzare sia il comportamento che le prestazioni in modo coerente.

<small> Fonte: [LinkedIn Learning: Microservices Foundations](https://www.linkedin.com/learning/microservices-foundations-23469069?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>  
<small> Fonte: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>