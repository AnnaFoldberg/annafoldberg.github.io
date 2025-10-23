---
title: "2025-10-23: Insights from Guild Meeting"
categories: [microservices, it-security]
tags: [guild, individual, reflection]
lang: en
locale: en
nav_order: 13
ref: log-2025-10-23-guild-meeting-insights
---
##### Microservices and Kubernetes  
**Logging and Security in Microservices**  
We discussed how essential logging really is and how much it should be part of the project. I have decided to keep it at a theoretical level, as it’s important to understand how logging and tracing are used to gain insight and track errors in a distributed system, but I don’t think it’s necessary to implement it fully at this stage. Instead, I’m focusing on more concrete security aspects such as JWT, OAuth, and service meshes, as they play a more central role in the system’s overall security architecture.  

**Messaging**  
Regarding messaging, we agreed that it’s worth going a bit deeper here, since microservices rarely make sense without some form of communication mechanism. I’m using RabbitMQ for my part of the system, while other members of the project group may choose different technologies. The most important thing is that I can justify my choice.  

**Perspective**  
We also discussed whether a microservices architecture even makes sense for a project of this size. Many of the more complex elements only become truly valuable at a larger scale, where there are many independent services.  

**Spikes**  
We talked about using small spikes to try out new technologies in limited experiments. This approach makes it possible to gain hands-on experience and test ideas on a smaller scale before they are potentially implemented in the full project.  

##### IT Security  
**Scope**  
It was clarified that the focus for IT security remains within our own defined scope, and that we are not expected to cover topics beyond the learning objectives and the content included in our portfolios.  

**HashiCorp Vault**  
We discussed that HashiCorp Vault is easy to set up with Kubernetes, but that in production it should run on a separate server or in a virtual machine rather than in a regular container, as it requires direct system access and stable storage.  
For demonstration or prototype purposes, however, it can easily run in a container.  
HashiCorp supports the assignment of roles and tokens, which automatically expire or are deactivated when the corresponding roles are closed. It issues tokens only to users who need access, and through the audit log it is possible to see who has accessed which resources.  

**Suggested Kubernetes Course**  
[Certified Kubernetes Administrator (CKA) – LinkedIn Learning](https://www.linkedin.com/learning/certified-kubernetes-administrator-cka-cert-prep-25818035/lesson-11-lab-solution-setting-up-storage?u=57075649)