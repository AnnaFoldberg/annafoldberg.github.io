---
title: "2025-09-18: Insights from Guild Meeting"
categories: [microservices, kubernetes]
tags: [guild, individual, reflection]
lang: en
locale: en
nav_order: 9
ref: log-2025-09-18-guild-meeting-insights
---
##### Microservices and Kubernetes  
**GraphQL server and service structure**  
We discussed that in a smaller system it probably makes the most sense to stick with a single GraphQL server, even though this creates a single point of failure. Adding more servers would only introduce unnecessary complexity and becomes relevant only in larger solutions.  
We also concluded that it still makes sense to keep the analysis service, even though the GraphQL server already routes client calls to the right microservices. The difference is that while the GraphQL server only handles routing from the client, the analysis service can hold business logic (e.g., how the system should react if an image is missing), which should not be placed in the GraphQL server.  

**Authentication with proxy**  
I clarified that an OAuth proxy can be a valid solution when Kong OSS does not support OIDC directly.  

**Documentation**  
We also talked about the importance of documentation in a microservices setup. While the team does not need to know all the technical details, it is important that they understand which calls and parameters to use. We should work continuously to keep the documentation clear and accessible, while maintaining an overview ourselves as the system grows.  

**Messaging and synchronicity**  
We also discussed message brokers, and how they both ensure asynchronous communication, so that sender and receiver do not need to be online at the same time, and, to some extent, can preserve the order of messages, for example within a queue or partition, even when services are temporarily down.  