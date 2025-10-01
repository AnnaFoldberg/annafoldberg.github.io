---
title: "Esecuzione di Stateful Workloads"
categories: [kubernetes]
tags: [databases, persistent-volumes]
lang: it
locale: it
nav_order: 53
ref: note-running-stateful-workloads
---
La gestione dell’archiviazione dei dati è una considerazione importante quando si eseguono applicazioni in Kubernetes. Gli stateful workloads possono essere gestiti in due modi principali: integrandosi con un database esterno oppure utilizzando Persistent Volumes all’interno del cluster, entrambi i quali garantiscono che i dati persistano oltre il ciclo di vita dei singoli Pods.

##### Database Esterno
Le applicazioni possono connettersi a un database in esecuzione al di fuori del cluster. Ad esempio, un’applicazione che utilizza PostgreSQL può fare affidamento su un database SQL autonomo ospitato su un server separato o su un managed service come Azure SQL, Amazon RDS o Google Cloud SQL. Il database viene configurato per comunicare con Kubernetes, pur rimanendo indipendente dal cluster.

##### Persistent Volumes
Kubernetes offre i Persistent Volumes per fornire uno storage che rimane disponibile anche dopo che un Pod è stato distrutto. Un StatefulSet può essere utilizzato per garantire che i Pods si colleghino costantemente allo stesso Persistent Volume, preservando i dati attraverso i riavvii o gli aggiornamenti dei Pods.

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>