---
title: "2025-11-30: The Role of Microservices and Kubernetes in the Project"
categories: [microservices, kubernetes, trackunit]
tags: [reflection, project, individual]
lang: en
locale: en
nav_order: 18
ref: log-2025-11-30-reflection-microservices-kubernetes
---

##### Microservices vs Monolith

The microservices architecture in the project provided a clear division of responsibilities, where each service had its own purpose and could be developed, deployed, and tested independently. This made it easier for the team to work in parallel and focus on separate functions without the risk that changes in one place blocked progress elsewhere in the system.

Given the current size of the system, however, a monolith could have been simpler and faster to develop. The system does not fully leverage the advantages of asynchronous communication, as the flow is essentially sequential. A monolith would therefore have reduced complexity without limiting functionality.

Microservices have thus primarily served as a learning tool, allowing me to work with service thinking, isolation, and scaling. It also supported team collaboration because responsibilities and boundaries were clearly defined. On the other hand, it required particular attention to integration and contracts between services to keep each microservice’s role clear.

##### Kubernetes vs docker-compose

In the prototype phase of the project, I used docker-compose to run the application and its infrastructure components. This provided a quick and simple environment where services could be started together on a single network.

The transition to Kubernetes via kind, however, introduced several significant operational advantages, including:

- **Automatic restart** of pods on failure  
- **Rolling updates** when new image versions are deployed  
- **Scaling** with more replicas in cases of higher load  
- **Network Policies** for controlling network traffic  
- **Namespaces** for isolating workloads  
- **Controlled entry point** via a LoadBalancer service, while internal services are kept inaccessible from outside with ClusterIP  
- **Helm charts** for stable and well-structured deployment setups of infrastructure components

Kubernetes provided the system with a more robust, scalable, and realistic operational model, with access to security mechanisms such as TLS, Ingress, secrets, and network policies, and made it possible to demonstrate real orchestration and security in a cloud-native environment.