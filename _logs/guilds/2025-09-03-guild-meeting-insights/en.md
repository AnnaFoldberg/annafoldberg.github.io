---
title: "2025-09-03: Insights from Guild Meeting"
categories: [microservices, kubernetes, it-security]
tags: [guild, individual, reflection]
lang: en
locale: en
nav_order: 7
ref: log-2025-09-03-guild-meeting-insights
---
**Microservices and Kubernetes**  
At the meeting, we discussed the choice of databases and considered whether PostgreSQL could be a good fit in our context. PostgreSQL is an object-relational database that can handle both classic relational data and also provides the ability to store objects directly. This raises the question of whether we should make use of this flexibility, or if it would instead introduce unnecessary complexity.  
Particularly in relation to microservices, it was noted that each microservice typically has its own database. If PostgreSQL is used here to store objects directly, we may end up with the same image or object stored in multiple databases, creating redundancy and increasing storage usage. In a more classical architecture with one common database combined with object storage, only the link to the object would be repeated, not the data itself. The reflection is therefore on whether PostgreSQL should be used as a common solution for both relational and object-like data in microservices, or whether in practice it is more appropriate to continue with relational databases combined with dedicated object storage.  

**IT Security**  
We were reminded of the importance of examining which regulations apply within our project and security domain and how they might influence our work and priorities. This is not only about technical implementation but also about ensuring that we continuously operate within the legal framework.