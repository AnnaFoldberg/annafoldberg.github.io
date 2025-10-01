---
title: "Tjek Pod Health med Event Logs"
categories: [kubernetes]
tags: [pods, health, logs]
lang: da
locale: da
nav_order: 42
ref: note-pod-health-event-logs
---
Begyndelsen af en Pods livscyklus er den mest sårbare fase. Fejl kan opstå, hvis container-imaget ikke er tilgængeligt, hvis der ikke er tilstrækkelig plads på worker nodes, eller hvis Pod’en starter men derefter crasher. Kubernetes gemmer event logs, når en Pod oprettes, og de kan bruges til fejlfinding.

##### Trin
**1. List Pods i et namespace:** Kør `kubectl get pods -n development`. Kopiér navnet på den Pod, der skal undersøges.  

**2. Beskriv Pod’en:** Kør `kubectl describe pod <pod-name> -n development`. Rul ned til bunden af outputtet for at se event logs.  

##### Fortolkning af events
- Hvis Pod’en er sund, vil events bekræfte normal scheduling og container-opstart.  
- Hvis Pod’en støder på problemer, vil fejlmeddelelser fremgå af events for at hjælpe med at identificere problemet.  
- Hvis Pod’en har kørt i et stykke tid uden problemer, vises der ingen nyere events.  

##### Hovedpointe
De fleste Pod-problemer opstår i det første minut af deres livscyklus. Derefter indikerer manglen på events som regel, at Pod’en er sund.

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>