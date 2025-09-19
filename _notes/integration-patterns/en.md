---
title: "Integration Patterns"
categories: [microservices]
tags: [architecture, patterns]
lang: en
locale: en
nav_order: 10
ref: note-integration-patterns
---
Integration patterns describe common ways to structure communication between microservices and clients. They provide strategies for routing, aggregating, or isolating requests, depending on the needs of the system.

**Gateway Pattern**

The gateway pattern acts as the ingress point for client communication. It provides a buffer between client needs and the underlying services, isolating the system and exposing well-defined APIs.

A gateway can proxy requests, decorate payloads, and perform simple aggregations. This avoids pushing complexity into the client while keeping the gateway itself lightweight.

Design strategy for gateways:

1. Define contracts  
2. Expose APIs through the gateway  
3. Apply strict version control  
4. Implement the gateway component  

**Process Aggregator Pattern**

The process aggregator pattern is used when several business processes must be called together and combined into a single response. Instead of requiring each client to manage multiple calls and replicate logic, the aggregator provides one API endpoint.

While useful, aggregators can become choke points or cause blocking calls if not carefully designed. They should only be used where the combined response is truly required.

Steps to design an aggregator:

1. Determine the business processes to combine  
2. Define the processing rules  
3. Create a new model for the composite payload  
4. Implement the API based on that model  
5. Wire the service and apply the processing logic  

**Edge Pattern**

The edge pattern addresses scalability and client-specific needs. Unlike a gateway, which serves all clients, an edge service focuses on a single client or a small set of clients with unique requirements.

By applying gateway-like transformations at the edge, the system avoids overloading the main gateway and gains flexibility for new client integrations.

Design strategy for edge services:

1. Identify client needs and constraints  
2. Build contracts  
3. Implement contracts  
4. Maintain the service as long as the client is supported  

**Edge vs. Gateway**

- **Edge:** Targets individual clients, applies gateway patterns in a client-specific way, more scalable and flexible.  
- **Gateway:** Central entry point for all clients, simpler to maintain, but less adaptable for unique client needs.  

<small> Source: [LinkedIn Learning: Solving Microservices Problems with Patterns](https://www.linkedin.com/learning/microservices-design-patterns-23454771/solving-microservices-problems-with-patterns?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>