---
title: "Visning af Application Logs"
categories: [kubernetes]
tags: [logs, debugging, pods]
lang: da
locale: da
nav_order: 44
ref: note-viewing-application-logs
---
En anden måde at verificere, at en applikation fungerer i Kubernetes, er ved at inspicere de logs, der genereres af dens Pods. Applikations-logs giver indblik i aktivitet under kørsel og er ofte afgørende for fejlfinding.

##### Trin
1. **List Pods i et namespace:** Kør `kubectl get pods -n development`. Kopiér navnet på den Pod, hvis logs skal inspiceres.  
2. **Vis logs:** Kør `kubectl logs <pod-name> -n development`. Dette udskriver log-outputtet fra den valgte Pod.  

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>