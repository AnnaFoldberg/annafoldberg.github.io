---
title: "Microservice Security Challenges"
categories: [microservices, it-security]
tags: [access-control, attack-surface]
lang: en
locale: en
nav_order: 17
ref: note-microservice-security-challenges
---
**Tiered Architecture Security:**

- Limited attack surface  
- Traffic interceptor  
- Shared security context  

**Microservice Architecture:**

- Disadvantages:  
    - Wide attack surface  
    - Dynamic attack surface (when we scale a service or a new service is introduced)  
    - Challenging authentication  
    - Crossing trust domains  
- Advantages:  
    - Improved availability (DoS-attack protection)  
    - Impact of breaches limited  

<small> Source: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>