---
title: "API Gateways"
categories: [microservices, it-security]
tags: [access-control, api-gateway, reverse-proxy, api-security]
lang: en
locale: en
nav_order: 19
ref: note-api-gateways
---
A reverse proxy acts as a front end through which all traffic to one or more servers passes. It provides a single entry point, hiding the details of the underlying servers and centralizing functionality at the gateway.

In microservices, an API gateway is a common type of reverse proxy. Placed at the edge of the deployment, it ensures that all traffic to the services flows through a single API interface. This creates a secure access point and shields clients from changes behind the gateway.

Additional gateway capabilities often include request tracing, performance monitoring, and API monetization. While features may overlap with those of IAM platforms, the reverse proxy remains the central mechanism for controlled access to microservices.

<small> Source: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>