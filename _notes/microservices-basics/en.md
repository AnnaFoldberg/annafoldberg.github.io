---
title: "Microservices Basics"
categories: [microservices]
tags: [overview, architecture, tools]
lang: en
locale: en
nav_order: 1
ref: note-microservices-basics
---
Each microserivce deals with one business function end-to-end independently from other microservices. They communicate with each other through lightweight common protocols such as HTTP or message queues. Thereby, they allow use of different programming languages for each microservice.  
However, it’s important to be aware that the more microservices, the more complexity. For example, if one microservice instance fails in production, it could generate a cascade of outages within other microservices, and it can be difficult to identify the root cause and fix it in a timely manner.

##### Tools
- **Containerization:** Helps deploy microservices in minimalist, self-contained runtimes (docker)
- **Container Orchestration:** Manages containers life cycles (kubernetes)
- **Pipeline Automation:** CI/CD (GitLab)
- **Asynchronous Messaging:** Further decouples microservices by providing message brokers and queues (kafka)
- **Performance Monitoring:** Tracks microservice performance (prometheus)
- **Logging and Audit:** Help keep track of everything happening within the system (datadog)