---
title: "Latency and Gridlock"
categories: [microservices]
tags: [architecture, reliability]
lang: en
locale: en
nav_order: 15
ref: note-latency-gridlock
---
Every service call introduces some latency. For a single call this is usually negligible, but as request paths grow more complex, the combined latency can become noticeable for the system.

If not managed, latency may accumulate and lead to gridlock, where parts of the system are unable to proceed. Gridlock can also occur when circular dependencies are introduced, for example when a service indirectly calls itself through other services.

To mitigate this, it is important to implement timeout logic across the system so that latency or circular calls do not propagate indefinitely.

**Circuit breaker**

A circuit breaker is a common pattern used to handle latency and gridlock. In this pattern, when timeouts occur, the circuit is opened and a fallback behavior is executed instead of repeatedly calling the affected service. This allows the system to continue operating with limited functionality rather than failing entirely. When the affected services recover, the circuit closes and the system resumes normal operation.

<small> Source: [LinkedIn Learning: Microservices Foundations](https://www.linkedin.com/learning/microservices-foundations-23469069?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>