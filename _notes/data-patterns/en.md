---
title: "Data Patterns"
categories: [microservices]
tags: [architecture, patterns]
lang: en
locale: en
nav_order: 11
ref: note-data-patterns
---
Data patterns describe common approaches to handling storage and transactions in microservices. The choice of pattern affects scalability, consistency, and how services interact with each other at the data layer. Some patterns focus on database design, while others address how data changes flow through a distributed system.

**Single Service Database**

Each service has its own dedicated database, aligned with its data domain. As the load on a service increases, its database scales with it. This isolation makes it possible to scale services and datastores independently, or even by region, without affecting the rest of the system.

The limitation is when domains are involved in atomic transactions. In those cases, the database boundaries may need to be less fine-grained to support ACID guarantees.

**Shared Service Database**

Sometimes services share a database, often due to enterprise contracts or startup constraints. In this pattern, data distribution is handled by the database engine itself through schemas, keyspaces, or other logical partitions.

Each service should have its own credentials and access only the schema it owns. Proper segmentation allows for eventual migration to fully isolated databases, since the logical separation already exists.

**Command Query Responsibility Segregation (CQRS)**

CQRS separates read and write operations into different models. Queries can transform or aggregate data into views optimized for consumption, while commands follow business rules for state changes.

This pattern is especially useful for task-based user interfaces and event-driven systems. However, it requires careful design, as poor implementations often lead to failures. When done correctly, it provides powerful flexibility in distributed environments.

**Asynchronous Eventing**

Some workflows are too long or complex for a single blocking API call. Asynchronous eventing addresses this by decoupling the request from the processing.

The service API triggers an event and immediately acknowledges the client, while the actual work proceeds in the background through a series of asynchronous actions. This model is particularly effective in distributed systems where long-running transactions or chained operations are required.

<small> Source: [LinkedIn Learning: Solving Microservices Problems with Patterns](https://www.linkedin.com/learning/microservices-design-patterns-23454771/solving-microservices-problems-with-patterns?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>