---
title: "Individual Learning Goals: Microservices & Kubernetes (15 ECTS)"
categories: [microservices, kubernetes]
tags: [individual]
lang: en
locale: en
nav_order: 21
ref: learning-goals-microservices-kubernetes
---

##### Knowledge  
I have:  
- Insight into different service types in a microservices system, including domain-based, business-process-based, and atomic transaction-based microservices.  
- Knowledge of key integration patterns such as the API Gateway Pattern, Aggregator Pattern, and Edge Pattern.  
- Knowledge of operational patterns, including the Log Aggregation Pattern, Metrics Aggregation Pattern, Tracing Pattern, external configuration, and service discovery.  
- Understanding of architectural differences between microservices and monoliths, including how the architectural choice affects scalability, complexity, fault isolation, and overall suitability for the specific project.  
- Knowledge of Kubernetes failure-handling and diagnostic mechanisms, including `kubectl logs` and `kubectl describe`.  
- Understanding of health, readiness, and liveness mechanisms in microservices and Kubernetes.  
- Knowledge of Kubernetes architecture, including control plane components (etcd, kube-apiserver, scheduler, controller-manager) and node components (kubelet, kube-proxy, container runtime).  
- Understanding of the differences between relevant Kubernetes service types, including ClusterIP, NodePort, and LoadBalancer.  
- Knowledge of the advantages of Helm charts over traditional Kubernetes manifests, including the use of values files for consistent and scalable configuration.  

##### Skills  
I can:  
- Assess and select appropriate service types, integration patterns, and operational patterns to solve specific problems.  
- Configure and use RabbitMQ as a message broker via the AMQP protocol for event- and command-based communication.  
- Use and adapt existing Helm charts via values files.  
- Set up and administer local Kubernetes clusters with kind.  
- Configure rolling update strategies in Kubernetes using maxSurge and maxUnavailable settings to support zero-downtime deployments.  
- Use versioned container images to achieve version-controlled updates and efficient rollback handling.  
- Use `kubectl logs` and `kubectl describe` for troubleshooting pods and deployments in Kubernetes.  

##### Competencies  
I can:  
- Design and plan the architecture of a microservices-based system with a focus on loosely coupled services, asynchronous communication, and scalability.  
- Administer and maintain Kubernetes environments based on kind, including working with deployments, services, secrets, namespaces, and network policies.