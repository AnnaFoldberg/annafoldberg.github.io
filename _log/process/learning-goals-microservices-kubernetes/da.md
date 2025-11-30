---
title: "Individuelle Læringsmål: Microservices & Kubernetes (15 ECTS)"
categories: [microservices, kubernetes]
tags: [individual]
lang: da
locale: da
nav_order: 21
ref: learning-goals-microservices-kubernetes
---

**Viden**

Jeg har:  
- Indsigt i forskellige servicetyper i et microservices-system, herunder domæne-, forretningsproces- og atomisk transaktionsbaserede microservices.  
- Kendskab til centrale integrationsmønstre som API Gateway Pattern, Aggregator Pattern og Edge Pattern.  
- Viden om operationelle mønstre, herunder Log Aggregation Pattern, Metrics Aggregation, Tracing Pattern, ekstern konfiguration og service discovery.  
- Forståelse for arkitekturforskelle mellem microservices og monolitter, herunder hvordan arkitekturvalget påvirker skalerbarhed, kompleksitet, fejlisolering og den samlede egnethed i forhold til det konkrete projekt.  
- Viden om Kubernetes’ fejlhåndterings- og diagnosemekanismer som `kubectl logs` og `kubectl describe`.  
- Forståelse for health-, readiness- og liveness-mekanismer i microservices og Kubernetes.  
- Viden om Kubernetes’ arkitektur, herunder control-plane komponenter (etcd, kube-apiserver, scheduler, controller-manager) og worker-node komponenter (kubelet, kube-proxy, container runtime).  
- Forståelse for forskellen mellem relevante Kubernetes-servicetyper (ClusterIP, NodePort, LoadBalancer).  
- Viden om fordelene ved Helm charts i forhold til manuel deployment, herunder skabelonstyring og genbrug af konfiguration til mere konsistente og skalerbare Kubernetes-deployments.

**Færdigheder**

Jeg kan:  
- Vurdere og udvælge passende servicetyper og integrationsmønstre til løsning af konkrete problemstillinger.  
- Konfigurere og benytte RabbitMQ som message broker vha. AMQP-protokollen til event- og kommandobaseret kommunikation.  
- Anvende og tilpasse Helm charts, inkl. values-filer og secrets, til deployment i lokale Kubernetes-clustre.  
- Opsætte og administrere lokale Kubernetes-clustre med Kind.  
- Konfigurere rolling update-strategier i Kubernetes ved hjælp af versionerede container-images samt indstillinger som `maxSurge` og `maxUnavailable` for at understøtte zero-downtime deployments.  
- Anvende `kubectl logs` og `kubectl describe` til troubleshooting af pods og deployments i Kubernetes.

**Kompetencer**

Jeg kan:  
- Designe og tilrettelægge arkitekturen for et microservices-baseret system med fokus på løst koblede services, asynkron kommunikation og skalering.  
- Administrere og vedligeholde Kubernetes-miljøer baseret på Kind, herunder arbejde med deployments, namespaces, network policies, services og secrets.