---
title: "Verifica Pod Health con Event Logs"
categories: [kubernetes]
tags: [pods, health, logs]
lang: it
locale: it
nav_order: 42
ref: note-pod-health-event-logs
---
L’inizio del ciclo di vita di un Pod è la fase più fragile. I problemi possono verificarsi se l’image del container non è disponibile, se non c’è spazio sufficiente sui worker nodes o se il Pod si avvia ma poi va in crash. Kubernetes salva gli event logs quando un Pod viene creato, che possono essere utilizzati per il troubleshooting.

##### Passaggi
**1. Elencare i Pods in un namespace:** Esegui `kubectl get pods -n development`. Copia il nome del Pod da analizzare.  

**2. Descrivere il Pod:** Esegui `kubectl describe pod <pod-name> -n development`. Scorri fino in fondo all’output per visualizzare gli event logs.  

##### Interpretazione degli eventi
- Se il Pod è in salute, gli eventi confermeranno il normale scheduling e l’avvio del container.  
- Se il Pod incontra problemi, nei log appariranno messaggi di errore utili a identificare il problema.  
- Se il Pod è in esecuzione da un po’ senza problemi, non verranno mostrati eventi recenti.  

##### Punto chiave
La maggior parte dei problemi dei Pod si verifica nel primo minuto del loro ciclo di vita. Successivamente, la mancanza di eventi indica solitamente che il Pod è in salute.

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>