---
title: "Modi per Gestire i Kubernetes Pods"
categories: [kubernetes]
tags: [pods, controllers]
lang: it
locale: it
nav_order: 52
ref: note-manage-kubernetes-pods
---
Kubernetes fornisce diversi controller per gestire i Pods, ciascuno progettato per casi d’uso specifici. I Deployments, i DaemonSets e i Jobs rappresentano tre strategie chiave: garantire la disponibilità continua, eseguire agenti su ogni node oppure svolgere workloads che devono completarsi.

##### Deployment
Il modo più comune per eseguire applicazioni containerizzate. I Deployments controllano il numero di repliche e supportano i rolling updates. Quando viene distribuita una nuova versione di un’applicazione, Kubernetes mantiene in esecuzione la vecchia versione, distribuisce la nuova, verifica la Pod health e poi rimuove i vecchi Pods. Questo consente aggiornamenti senza downtime.

##### DaemonSet
Garantisce che una copia di un Pod venga eseguita su ogni node del cluster. Il numero di repliche non può essere controllato direttamente. I DaemonSets sono tipicamente utilizzati per processi in background, come agenti che raccolgono metrics su nodes e Pods.

##### Job
Esegue uno o più Pods fino a quando un’attività specificata non viene completata con successo. I Jobs sono utili per workloads che devono completarsi, come la generazione di dati di test in un processo batch. Una volta completati, i Pods possono essere eliminati.

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>