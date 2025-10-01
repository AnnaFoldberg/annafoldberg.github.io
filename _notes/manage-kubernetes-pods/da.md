---
title: "Måder at Administrere Kubernetes Pods"
categories: [kubernetes]
tags: [pods, controllers]
lang: da
locale: da
nav_order: 52
ref: note-manage-kubernetes-pods
---
Kubernetes tilbyder flere controllere til at administrere Pods, hver designet til specifikke use cases. Deployments, DaemonSets og Jobs repræsenterer tre centrale strategier: at sikre kontinuerlig tilgængelighed, at køre agenter på hver node eller at udføre workloads, der skal køre færdig til afslutning.

##### Deployment
Den mest almindelige måde at køre containeriserede applikationer på. Deployments styrer antallet af replikaer og understøtter rolling updates. Når en ny version af en applikation deployes, holder Kubernetes den gamle version kørende, ruller den nye ud, verificerer Pod health og fjerner derefter de gamle Pods. Dette muliggør opgraderinger uden nedetid.

##### DaemonSet
Sikrer, at én kopi af en Pod kører på hver node i clusteret. Antallet af replikaer kan ikke styres direkte. DaemonSets bruges typisk til baggrundsprocesser, såsom agenter, der indsamler metrics om nodes og Pods.

##### Job
Kører en eller flere Pods, indtil en specificeret opgave fuldføres med succes. Jobs er nyttige til workloads, der skal køre til afslutning, såsom at generere testdata i en batch-proces. Når de er færdige, kan Pods slettes.

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>