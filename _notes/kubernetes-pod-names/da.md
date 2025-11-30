---
title: "Kubernetes Pod Navne"
categories: [kubernetes]
tags: [pods, naming, replicasets, deployments]
lang: da
locale: da
nav_order: 58
ref: note-kubernetes-pod-names
---

Et typisk Kubernetes pod-navn kan se sådan her ud: `myapp-7c9d7bc5b8-2xkpf`. Det består af tre dele, som hver har en specifik betydning.

1. `myapp`: Navnet fra Deployment-objektet. Alle pods, som et deployment opretter, starter med dette navn.
2. `7c9d7bc5b8`: ReplicaSet-hash. Dette er et hash Kubernetes genererer for at identificere det ReplicaSet, der hører til den nuværende version af Deploymentet. Hver gang spec.template ændres, oprettes et nyt ReplicaSet med et nyt hash.
3. `2xkpf`: Pod-ID. Dette er et unikt genereret suffix, som Kubernetes tilføjer for at sikre, at to pods aldrig får samme navn. Det identificerer den enkelte pod.

Man kan altså have følgende pods, som tilhører samme Deployment og ReplicaSet, men som hver har deres eget unikke Pod-ID:

```yaml
myapp-7c9d7bc5b8-2xkpf
myapp-7c9d7bc5b8-hlsbr
myapp-7c9d7bc5b8-rrlqt
```