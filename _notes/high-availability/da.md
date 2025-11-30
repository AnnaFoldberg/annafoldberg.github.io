---
title: "Høj Tilgængelighed i Kubernetes"
categories: [kubernetes]
tags: [high-availability, pods, nodes, scaling]
lang: da
locale: da
nav_order: 66
ref: note-high-availability
---

Kubernetes er en container-orchestrator, der håndterer deployment og drift af mange containere på tværs af et cluster.

##### Pods

Når Kubernetes kører en applikation, gør den det ved at oprette pods.

En pod er en kørende instans af en applikation. Der kan eksistere flere pods af samme applikation for at sikre høj tilgængelighed.

##### Noder

Pods kører på noder. En node er en maskine i clusteret, for eksempel en virtuel server. Et cluster består af flere noder. Én node kan:

- Køre pods fra mange forskellige applikationer  
- Køre flere pods fra den samme applikation  

Kubernetes bestemmer placeringen dynamisk.

##### Høj tilgængelighed

Kubernetes gør applikationer højtilgængelige, fordi pods kan fordeles på flere noder.

Det betyder:

- Hvis en enkelt pod fejler → Kubernetes genstarter den.  
- Hvis en hel node fejler (maskinen går ned) → alle pods på den node forsvinder.  
- Pods på de øvrige noder fortsætter uforstyrret.  
- Kubernetes starter automatisk nye pods på de noder, der stadig virker.  

På den måde fortsætter applikationen med at køre, selv hvis en hel maskine i clusteret går ned.

##### Skalering og drift

Kubernetes fordeler automatisk trafik mellem pods og kan hurtigt starte flere pods, når belastningen stiger.  
Man kan også tilføje eller fjerne worker-noder uden at afbryde applikationen.  
Det hele styres gennem deklarative konfigurationsfiler (manifester), som gør drift og administration forudsigelig og automatiserbar.

<small>Kilde: [Udemy: Learn Kubernetes](https://www.udemy.com/course/learn-kubernetes/)</small>