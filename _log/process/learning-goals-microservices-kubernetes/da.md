---
title: "Individuelle Læringsmål: Microservices & Kubernetes (15 ECTS)"
categories: [microservices, kubernetes]
tags: [individual]
lang: da
locale: da
nav_order: 21
ref: learning-goals-microservices-kubernetes
---

##### Viden  
Jeg har:  
- Indsigt i forskellige servicetyper i et microservices-system, herunder domæne-, forretningsproces- og atomisk transaktionsbaserede microservices.  
- Kendskab til centrale integrationsmønstre som API Gateway Pattern, Aggregator Pattern og Edge Pattern.  
- Viden om operationelle mønstre, herunder Log Aggregation Pattern, Metrics Aggregation Pattern, Tracing Pattern, ekstern konfiguration og service discovery.  
- Forståelse for arkitekturforskelle mellem microservices og monolitter, herunder hvordan arkitekturvalget påvirker skalerbarhed, kompleksitet, fejlisolering og den samlede egnethed i forhold til det konkrete projekt.  
- Viden om Kubernetes’ fejlhåndterings- og diagnosemekanismer, herunder kubectl logs og kubectl describe.  
- Forståelse for health-, readiness- og liveness-mekanismer i microservices og Kubernetes.  
- Viden om Kubernetes’ arkitektur, herunder control-plane komponenter (etcd, kube-apiserver, scheduler, controller-manager) og node-komponenter (kubelet, kube-proxy, container runtime).  
- Forståelse for forskellen mellem relevante Kubernetes-servicetyper, herunder ClusterIP, NodePort og LoadBalancer.  
- Viden om fordelene ved Helm charts i forhold til traditionelle Kubernetes-manifester, herunder brug af values-filer til konsistent og skalerbar konfiguration.  

##### Færdigheder  
Jeg kan:  
- Vurdere og udvælge passende servicetyper, integrationsmønstre og operationelle mønstre til løsning af konkrete problemstillinger.  
- Konfigurere og benytte RabbitMQ som message broker via AMQP-protokollen til event- og kommandobaseret kommunikation.  
- Anvende og tilpasse eksisterende Helm charts via values-filer.  
- Opsætte og administrere lokale Kubernetes-clustre med kind.  
- Konfigurere rolling update-strategier i Kubernetes med maxSurge- og maxUnavailable-indstillinger for at understøtte zero-downtime deployments.  
- Anvende versionerede container-images til at opnå versionskontrollerede opdateringer og effektiv rollback-håndtering.  
- Anvende kubectl logs og kubectl describe til troubleshooting af pods og deployments i Kubernetes.  

##### Kompetencer  
Jeg kan:  
- Designe og tilrettelægge arkitekturen for et microservices-baseret system med fokus på løst koblede services, asynkron kommunikation og skalering.  
- Administrere og vedligeholde Kubernetes-miljøer baseret på kind, herunder arbejde med deployments, services, secrets, namespaces og network policies.  