---
title: "Edge Services"
categories: [microservices]
tags: [architecture]
lang: en
locale: en
nav_order: 14
ref: note-edge-services
---
There exist two distinct types of edge services:

**Inbound edge services** abstract third-party dependencies. External system contracts change frequently, and calling them directly spreads that impact across the entire platform. By introducing an edge service, all changes are managed in one place. This also creates flexibility if the external provider needs to be replaced in the future.

**Outbound edge services** handle client-specific needs. Different clients often require variations in payloads or transformations. Instead of pushing this logic into the API gateway or core services, outbound edge services manage these transformations while still providing a consistent interface to clients.

<small> Source: [LinkedIn Learning: Microservices Foundations](https://www.linkedin.com/learning/microservices-foundations-23469069?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>