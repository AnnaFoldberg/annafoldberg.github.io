---
title: "The API Layer"
categories: [microservices]
tags: [architecture]
lang: en
locale: en
nav_order: 13
ref: note-api-layer
---
The API layer acts as a pure proxy, exposing services through a single entry point without adding client-specific logic. It isolates clients from needing to know the IP address or port of the services they consume.

From the client’s perspective, all requests go to the API layer, which routes them to the appropriate service. This means the client is not impacted if the underlying microservices change, as the API layer manages any internal communication differences.

<small> Source: [LinkedIn Learning: Microservices Foundations](https://www.linkedin.com/learning/microservices-foundations-23469069?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>