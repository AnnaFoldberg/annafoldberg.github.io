---
title: "Throttling and Rate Limiting"
categories: [microservices, it-security]
tags: [rate-limiting, api-security, scalability]
lang: en
locale: en
nav_order: 30
ref: note-throttling-rate-limiting
---
Scaling is the first response when demand on a microservice grows, and container orchestrators support this with auto-scaling. But scaling has limits: resource costs, exhausted host capacity, or cases where scaling would be harmful, such as during denial-of-service attacks. For these situations, controlling consumption becomes critical.

- **Throttling** controls client usage by limiting traffic once thresholds are reached, preventing complete outages.  
- **Quotas** define the number of calls permitted over longer intervals, such as per day or month, and are often tied to monetized APIs.  
- **Rate limiting** focuses on concurrent calls, usually measured in transactions per second.  

Quota and throttling strategies can be applied at different levels. Some clients, such as first-party applications, may be given higher quotas than third-party clients. Granularity can also be applied: limits by user (based on end-user identity) or limits by operation (based on the resource cost of each API call).

These techniques protect microservices from overload and ensure fair and sustainable use across all clients.

<small> Source: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>