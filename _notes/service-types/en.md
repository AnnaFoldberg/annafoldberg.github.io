---
title: "Service Types"
categories: [microservices]
tags: [architecture, patterns]
lang: en
locale: en
nav_order: 9
ref: note-service-types
---
A microservices system typically consists of a combination of different service types. Three common patterns are domain-based services, business process-based services, and atomic transaction-based services. Together, they form the structure of the system and reflect the different levels at which services can be designed.

**Domain-based Microservices**

Domain-based microservices are the most common design pattern in microservices architecture. Building services around data domains makes them more scalable and easier to manage.

The decomposition is driven by the domain itself: the service owns its data, applies logic to that data, and exposes the necessary actions. The focus is on clear ownership of the domain model, not on the details of the database schema.

Domain-driven design is often used to decide service boundaries. A bounded context defines where a domain model applies with a consistent meaning. By identifying these contexts and their use cases, a large system can be split into services that align with real business domains. Analyzing traffic patterns helps refine the boundaries, reducing unnecessary cross-domain calls and the latency they create.

The process of building a domain-based service:

1. Model the domain  
2. Define the actions  
3. Design the datastore to support it  

This keeps the service self-contained and closely aligned with the domain it represents.

**Business process-based microservices**

Business process-based microservices provide a higher level of abstraction by encapsulating specific business functionality. They help reduce duplication when processes span multiple domains, simplifying the architecture and improving scalability.

These services usually do not manage their own data. Instead, they consume the necessary domain services and focus only on the orchestration of logic. This separation avoids mixing business processes with data ownership, keeping boundaries clear.

The process of building a business process service:

1. Identify the process to expose  
2. Identify the data domains it consumes  
3. Define the APIs, focusing on contracts rather than underlying models  
4. Encapsulate the business process logic in its own module  

This keeps the service independent, reusable, and aligned with higher-level business operations.

**Atomic Transaction-Based Microservices**

Atomic transaction-based microservices are needed when transactions must span multiple data domains. Their purpose is to guarantee ACID-compliant operations across services, ensuring that all changes succeed or all are rolled back together.

These services are blocking by design: the caller must receive either a confirmation of success or an error. While asynchronous patterns exist, most use cases demand immediate guarantees. For single-domain transactions, this specialization is unnecessary, since the underlying service can handle atomicity internally.

Design considerations for atomic transaction services:

1. Confirm whether the service truly needs atomic behavior  
2. Use a shared database across the involved domains (with caution, as this increases coupling)  
3. Define the transaction scope, including rollback conditions  
4. Implement with fast fail and rollback mechanisms  

These services are complex and introduce limitations in distribution and scalability. They should only be used when absolutely required, but when the need arises, they must be supported.

<small> Source: [LinkedIn Learning: Microservices Foundations](https://www.linkedin.com/learning/microservices-foundations-23469069?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>  
<small> Source: [LinkedIn Learning: Solving Microservices Problems with Patterns](https://www.linkedin.com/learning/microservices-design-patterns-23454771/solving-microservices-problems-with-patterns?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>