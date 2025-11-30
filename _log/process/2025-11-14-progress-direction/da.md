---
title: "2025-11-14: Fremskridt og Retning"
categories: [microservices, kubernetes, it-security]
tags: [individual, process, feedback, feed-forward]
lang: da
locale: da
nav_order: 17
ref: log-2025-11-14-progress-direction
---

##### Feedback

Jeg har nu flyttet Trackunit-miljøet fra docker-compose over i Kubernetes.

Jeg har oprettet deployments, services, secrets, namespaces, network policies samt Helm values-filer til brug i Helm charts. Alle microservices kører som pods styret af deployments og eksponeres internt via ClusterIP-services, mens kun Kong er eksponeret eksternt via en LoadBalancer-service. Infrastrukturkomponenter som **kong**, **rabbitmq** og **oauth2-proxy** kører som Kubernetes-ressourcer via Helm. Det samlede setup er dermed blevet mere fleksibelt, skalerbart og robust.

Jeg bruger `kind` til at køre mit lokale Kubernetes-cluster, da det giver et realistisk miljø og gør det muligt at teste Kongs LoadBalancer-service via `cloud-provider-kind`. Kong er den eneste service, der eksponeres uden for clusteret og fungerer som indgangspunkt til systemet.

Til styring og overvågning af clusteret bruger jeg `k9s`, som giver et hurtigt og effektivt overblik over deployments, pods, logs og hændelser.

##### Feed-forward

Mit næste fokus bliver at indføre versionering på container-images, så jeg bedre kan styre releases, og så Kubernetes automatisk kan udføre rene rolling updates ved opdatering til nye image-versioner.

Derefter vil jeg implementere TLS-terminering mellem klienten og **kong-proxy**, så al trafik uden for clusteret er krypteret under transport.  

Jeg vælger ikke at indføre mTLS, hverken ved at kræve certifikat på klienten eller internt mellem pods i Kubernetes via et service mesh, på grund af projektets størrelse og fordi intern trafik allerede er beskyttet udadtil af API-gatewayen. Selv i et produktionsmiljø ville en angriber skulle forbi både API-gatewayen og autorisationen i **graph-gateway**, før intern pod-til-pod kommunikation kan tilgås.  

Når de resterende microservices er implementeret, skal **svc-analysis-orchestrator** udbygges til at styre analyseflowet efter upload. Der skal også implementeres en messaging bridge, som fungerer som adapter mellem RabbitMQ og Kafka, da mine eksisterende microservices bruger RabbitMQ, mens de nye services benytter Kafka. **graph-gateway** skal samtidig udvides med GraphQL-endpoints, der videresender forespørgsler til tu-ingestion-service via dens REST-endpoints og håndterer de svar, der kommer retur.