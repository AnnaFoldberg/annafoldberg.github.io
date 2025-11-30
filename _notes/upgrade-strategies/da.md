---
title: "Opgraderingsstrategier i Kubernetes"
categories: [kubernetes]
tags: [deployments, rolling-update, recreate, replicasets]
lang: da
locale: da
nav_order: 62
ref: note-upgrade-strategies
---

Kubernetes har to strategier til at opgradere en applikation i et Deployment:

##### Recreate  
- Alle eksisterende pods slettes på én gang.  
- Når de gamle pods er fjernet, oprettes nye pods med den opdaterede version.  
- Applikationen har downtime, mens der ikke kører nogen pods.

##### RollingUpdate (default)  
RollingUpdate udskifter pods gradvist for at undgå downtime. Strategien styres af to værdier:  

- **maxSurge:** Hvor mange ekstra pods Kubernetes må oprette midlertidigt.  
- **maxUnavailable:** Hvor mange pods Kubernetes må tage ned ad gangen.  

Begge værdier kan angives som tal (f.eks. 1) eller procent (f.eks. 25%).  
Dette giver kontrollerede opgraderinger, hvor applikationen fortsætter med at køre, mens pods udskiftes én efter én.

Hvis man ikke angiver nogen strategi, bruger Kubernetes automatisk RollingUpdate med standardværdierne `maxSurge: 1` og `maxUnavailable: 1`.

##### Eksempel på RollingUpdate i praksis

![Deployment Structure](../../assets/images/notes/upgrade-strategies/deployment-structure.png)

Billedet viser standardstrukturen for et Deployment i drift.

![New ReplicaSet](../../assets/images/notes/upgrade-strategies/new-replicaset.png)

Når versionen ændres i Deploymentet, oprettes automatisk et nyt ReplicaSet med den opdaterede image-version. Kubernetes begynder derefter at udfase pods i det gamle ReplicaSet og oprette nye pods i det nye.

![Rolling Transition](../../assets/images/notes/upgrade-strategies/rolling-transition.png)

Med både `maxSurge` og `maxUnavailable` sat til 1 foregår opdateringen sådan: Kubernetes skalerer én gammel pod ned og én ny pod op ad gangen. De resterende pods i det gamle ReplicaSet forbliver funktionsdygtige, så applikationen fortsætter uden afbrydelse.

![Mixed Traffic Phase](../../assets/images/notes/upgrade-strategies/mixed-traffic.png)

I overgangsfasen fordeles trafikken mellem den gamle og den nye version. Applikationen fungerer stadig, selv om nogle brugere rammer den gamle version og andre den nye.

![Finalization](../../assets/images/notes/upgrade-strategies/finalization.png)

Når alle pods i den nye version kører, kan det gamle ReplicaSet fjernes. Kubernetes beholder som regel det gamle ReplicaSet en periode for at muliggøre hurtig rollback.