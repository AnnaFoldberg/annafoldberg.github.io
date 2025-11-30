---
title: "Individual Learning Goals: Microservices & Kubernetes (15 ECTS)"
categories: [microservices, kubernetes]
tags: [individual]
lang: en
locale: en
nav_order: 21
ref: learning-goals-microservices-kubernetes
---

**Knowledge**

I have:  
- Insight into different service types in a microservices system, including domain-based, business-process-based and atomic transaction-based microservices.  
- Knowledge of central integration patterns such as the API Gateway Pattern, Aggregator Pattern and Edge Pattern.  
- Knowledge of operational patterns, including the Log Aggregation Pattern, Metrics Aggregation, Tracing Pattern, external configuration and service discovery.  
- Understanding of architectural differences between microservices and monoliths, including how the architectural choice impacts scalability, complexity, fault isolation and overall suitability for a given project.  
- Knowledge of Kubernetes failure-handling and diagnostic mechanisms such as `kubectl logs` and `kubectl describe`.  
- Understanding of health, readiness and liveness mechanisms in both microservices and Kubernetes.  
- Knowledge of Kubernetes architecture, including control-plane components (etcd, kube-apiserver, scheduler, controller-manager) and worker-node components (kubelet, kube-proxy, container runtime).  
- Understanding of the differences between Kubernetes service types (ClusterIP, NodePort, LoadBalancer).  
- Knowledge of the advantages of Helm charts compared to manual deployments, including templating, configuration reuse and support for more consistent and scalable Kubernetes deployments.

**Skills**

I can:  
- Assess and select appropriate service types and integration patterns to solve concrete architectural problems.  
- Configure and use RabbitMQ as a message broker via the AMQP protocol for event- and command-based communication.  
- Use and adapt Helm charts, including values files and secrets, for deployment in local Kubernetes clusters.  
- Set up and manage local Kubernetes clusters using Kind.  
- Configure rolling update strategies in Kubernetes using versioned container images and settings such as `maxSurge` and `maxUnavailable` to support zero-downtime deployments.  
- Use `kubectl logs` and `kubectl describe` to troubleshoot pods and deployments.

**Competencies**

I am able to:  
- Design and plan the architecture of a microservices-based system with a focus on loose coupling, asynchronous communication and scalability.  
- Administer and maintain Kubernetes environments based on Kind, including working with deployments, namespaces, network policies, services and secrets.