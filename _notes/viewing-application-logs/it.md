---
title: "Visualizzazione degli Application Logs"
categories: [kubernetes]
tags: [logs, debugging, pods]
lang: it
locale: it
nav_order: 44
ref: note-viewing-application-logs
---
Un altro modo per verificare che un’applicazione funzioni in Kubernetes è ispezionare i logs generati dai suoi Pods. I logs delle applicazioni forniscono informazioni sull’attività in fase di esecuzione e sono spesso essenziali per il debugging.

##### Passaggi
1. **Elencare i Pods in un namespace:** Esegui `kubectl get pods -n development`. Copia il nome del Pod di cui vuoi ispezionare i logs.  
2. **Visualizzare i logs:** Esegui `kubectl logs <pod-name> -n development`. Questo stampa l’output dei log del Pod selezionato.  

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>