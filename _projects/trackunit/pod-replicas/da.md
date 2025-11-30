---
title: "Trackunit: Valg af Replicas"
categories: [trackunit, kubernetes]
tags: [pods, replicas, scaling, final]
lang: da
locale: da
nav_order: 46
ref: pod-replicas
---

##### Egne services

- **graph-gateway:** Kører med `3` pods, da dette er indgangspunktet til systemet. Tre replicas giver højere tilgængelighed ved pod-crash og bedre kapacitet til at håndtere spikes i trafik.  
- **svc-analysis-orchestrator:** Kører med `2` pods, da den er central for analyseflowet, men ikke ligger direkte i klientens request-path. To replicas giver fault tolerance uden at bruge lige så mange ressourcer som gatewayen.

##### Helm-deployede komponenter

- **Kong**, **oauth2-proxy** og **RabbitMQ:** Kører med `1` pod hver. Det er infrastrukturtjenester og ændres sjældent sammenlignet med egne services. Flere replicas ville øge kompleksiteten uden at tilføre væsentlig værdi i det nuværende setup. De er dog stadig single points of failure, eftersom store dele af systemet vil påvirkes, hvis et af dem er utilgængelige.

##### Opsummering

I Trackunit-projektet ville `1` pod pr. service i praksis være tilstrækkeligt, da systemet er lille, kører lokalt og kun servicerer en enkelt klient. De fleste komponenter er gensidigt afhængige, og et nedbrud i én service påvirker derfor uundgåeligt helheden.

Antallet af replicas på **graph-gateway** og **svc-analysis-orchestrator** er derfor primært valgt for at demonstrere, hvordan services kan skaleres efter deres rolle og kritikalitet i et mere produktionslignende miljø. Skaleringsvalgene viser, hvordan man kan øge tilgængelighed og robusthed for de dele af systemet, hvor det er mest meningsfuldt, mens infrastrukturen holdes enkel.