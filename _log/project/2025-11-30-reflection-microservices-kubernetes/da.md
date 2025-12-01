---
title: "2025-11-30: Microservices og Kubernetes' Rolle i Projektet"
categories: [microservices, kubernetes, trackunit]
tags: [reflection, project, individual]
lang: da
locale: da
nav_order: 18
ref: log-2025-11-30-reflection-microservices-kubernetes
---

##### Microservices kontra monolit

Microservices-arkitekturen har i projektet givet en tydelig ansvarsfordeling, hvor hver service har sit eget formål og kan udvikles, deployes og testes uafhængigt. Det har gjort det lettere at arbejde parallelt i teamet og fokusere på separate funktioner uden risiko for, at ændringer ét sted blokerede arbejdet andre steder i systemet.

Set ud fra systemets aktuelle størrelse kunne en monolit dog have været enklere og hurtigere at udvikle. Systemet udnytter ikke fuldt ud fordelene ved asynkron kommunikation, da flowet i praksis er sekventielt. En monolit ville derfor have reduceret kompleksiteten uden at begrænse funktionaliteten.

Microservices har dermed primært fungeret som et læringsredskab, hvor jeg har arbejdet med servicetænkning, isolation og skalering. Det har samtidig understøttet teamets samarbejde, fordi ansvar og grænseflader var klart afgrænsede. Til gengæld har det krævet særlig opmærksomhed på integration og kontrakter mellem services, så hver microservices rolle forblev tydelig.

##### Kubernetes kontra docker-compose

I projektets prototypefase anvendte jeg docker-compose til at køre applikationen og dens infrastrukturkomponenter. Det gav et hurtigt og enkelt miljø, hvor services kunne startes samlet på ét netværk.

Overgangen til Kubernetes via kind tilførte derimod en række væsentlige driftsmæssige fordele, herunder:

- **Automatisk genstart** af pods ved fejl  
- **Rolling updates**, når nye image-versioner deployes  
- **Skalering** med flere replicas, hvor der kan opstå højere belastning  
- **Network Policies** til kontrol over netværkstrafik  
- **Namespaces** til isolering af workloads  
- **Kontrolleret indgang** via en LoadBalancer-service, mens interne services holdes utilgængelige udefra med ClusterIP  
- **Helm charts** til stabile og gennemarbejdede deployment-opsætninger af infrastrukturkomponenter

Kubernetes gav systemet en mere robust, skalerbar og realistisk driftsmodel med adgang til sikkerhedsmekanismer som TLS, Ingress, secrets og network policies, og gjorde det muligt at demonstrere reel orkestrering og sikkerhed i et cloud-native miljø.