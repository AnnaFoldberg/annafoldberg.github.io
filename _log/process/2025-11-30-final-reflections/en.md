---
title: "2025-11-30: Final Reflections"
categories: [microservices, kubernetes, it-security]
tags: [individual, reflection]
lang: en
locale: en
nav_order: 18
ref: log-2025-11-30-final-reflections
---

##### Initial Learning Goals

**Microservices and Kubernetes**  
At the beginning of the semester, my goal was to gain a deeper understanding of how microservices and Kubernetes can be used in practice to build scalable and flexible systems. I wanted to be able to design an architecture where components are well-defined, loosely coupled, and can be developed and operated independently. Kubernetes would then ensure stability, scalability, and operational reliability.

**IT Security**  
I aimed to work with security as an integrated part of the architecture. My goal was to identify key security risks in a microservices environment with Kubernetes and select appropriate mechanisms to protect data, communication, and infrastructure.

##### Process

I began the semester by gathering and organising information, as I had no prior experience with these technologies. I followed learning paths on LinkedIn Learning covering microservices, Kubernetes, and related IT security, which gave me a broad understanding of essential concepts from design, operational, and security perspectives. At the same time, I needed a quick understanding of microservices and message brokers so the team could choose a sensible architecture early on.

As other team members also wanted to explore microservices concepts and messaging to varying degrees, we divided responsibilities across the system. I took ownership of the API gateway, the GraphQL server, and the central analysis orchestration. This allowed those focusing more on backend development to work on the logic-heavy services. The division also reflected the system’s two main flows: image upload and image analysis.

After familiarising myself with microservices principles, RabbitMQ, and GraphQL, I created a spike project, **TeaApp**, where I experimented with microservices architecture, authentication via Microsoft Entra ID, Kong as an API gateway, GraphQL operations, and event publication using RabbitMQ. This provided practical experience and a strong foundation for the Trackunit project.

I then continued with the Kubernetes learning path on LinkedIn Learning but quickly shifted to Kubernetes’ official documentation, which I found easy to translate directly into practice. Alongside this, I used Udemy courses to understand more complex relationships, particularly around security and interactions between Kubernetes components.

I had also planned to explore OWASP API Security Top 10, OWASP Kubernetes Top 10, and the Cyber Security Roadmap. Although I consulted them occasionally, I chose not to dive deeply into them, preferring to focus on fewer but essential security mechanisms consistently highlighted across courses and articles.

##### Trackunit Prototype in Docker

I began implementing the Trackunit prototype with only the services I was personally responsible for: the API gateway (**kong** and **oauth2-proxy**), **graph-gateway**, and **svc-analysis-orchestrator**.

**InfraCore** grouped all services and infrastructure components into a single docker-compose environment so everything could run together on a shared Docker network. I also implemented a **ClientConsole** to test authentication, the GraphQL flow, and verify that events were being published correctly.

During this phase, the team wanted to switch communication from RabbitMQ to Kafka. I experimented with this but chose to keep RabbitMQ as my message broker due to my stronger foundational understanding of it and my prioritisation of other areas. As the team continued with Kafka, I planned to later introduce a **messaging bridge** capable of converting Kafka events into RabbitMQ events. It was originally intended to work bidirectionally, but the team decided that **svc-ai-vision-adapter** should receive `tu.image.uploaded` directly from **tu-ingestion-service**, bypassing **svc-analysis-orchestrator**. This reduced the orchestrator’s architectural role to more of a notifier, though I kept the service because it would be essential in a more scalable version of the system.

##### Transition to Kubernetes

I then migrated the system from docker-compose to Kubernetes. I chose to run everything locally in `kind`, as my goal was to learn Kubernetes operations and security mechanisms rather than deploy to cloud. I selected specific Kubernetes resources that address both operations and security: deployments, services, secrets, namespaces, and network policies.

I deployed infrastructure components using Helm charts with values files to ensure stable, consistent, and security-minded configurations. This provided a **kong-proxy** LoadBalancer service accessible via `cloud-provider-kind`.

For cluster management and monitoring, I used `k9s`, which gave me an efficient overview of components and logs.

I then worked with key and certificate management to establish proper TLS termination between the client and **kong-proxy**. As part of this, I planned to extend the setup so that **kong** would serve not only as an API Gateway but also as an Ingress Controller, with an Ingress resource defining routing and TLS configuration for incoming external traffic. This ensures that all traffic outside the cluster is encrypted in transit and that certificates terminate securely at **kong-proxy**.

I chose not to introduce mTLS — neither between the client and **kong-proxy**, nor internally between pods via a service mesh. Such a solution would be excessive given the scope of the project and the intended learning outcomes, but it could be meaningfully prioritised in a later iteration focused on a more comprehensive security model.

##### Integration with External Services

Before integrating the team’s microservices, I introduced versioning of container images. This allowed me to roll back deployments easily if new image versions caused unexpected issues in communication between services.

When integrating the team’s microservices with my cluster, I chose not to deploy their services into Kubernetes. Setting up additional components such as **Redpanda**, **MinIO**, and a **Tailscale sidecar**, alongside the team’s microservices, would introduce significant complexity without proportional learning value at this stage. Since the Trackunit project serves as a proof-of-concept, I prioritised Kubernetes operations and security. These services could naturally be migrated into the cluster in a future iteration.

As part of the integration, I implemented **svc-messaging-bridge**, which provided the connection between Kafka and RabbitMQ.

##### Summary

Throughout this process, I achieved the learning goals I set at the beginning of the semester. This has resulted in concrete, individual learning outcomes mapped to Bloom’s taxonomy, giving a clear overview of the theoretical and practical levels at which I have worked.