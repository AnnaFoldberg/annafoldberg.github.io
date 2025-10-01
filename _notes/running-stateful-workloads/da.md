---
title: "Kørsel af Stateful Workloads"
categories: [kubernetes]
tags: [databases, persistent-volumes]
lang: da
locale: da
nav_order: 53
ref: note-running-stateful-workloads
---
Håndtering af datalagring er en vigtig overvejelse, når man kører applikationer i Kubernetes. Stateful workloads kan administreres på to hovedmåder: ved at integrere med en ekstern database eller ved at bruge Persistent Volumes i clusteret, som begge sikrer, at data bevares ud over den enkelte Pods livscyklus.

##### Ekstern Database
Applikationer kan oprette forbindelse til en database, der kører uden for clusteret. For eksempel kan en applikation, der bruger PostgreSQL, enten benytte en selvstændig SQL-database hostet på en separat server eller en managed service såsom Azure SQL, Amazon RDS eller Google Cloud SQL. Databasen konfigureres til at kommunikere med Kubernetes, mens den forbliver uafhængig af clusteret.

##### Persistent Volumes
Kubernetes tilbyder Persistent Volumes til at levere lagring, der forbliver, selv efter en Pod er blevet destrueret. En StatefulSet kan bruges til at sikre, at Pods konsekvent forbinder til det samme Persistent Volume, så data bevares på tværs af Pod-genstarter eller opdateringer.

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>