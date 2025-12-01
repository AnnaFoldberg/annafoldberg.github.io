---
title: "2025-11-06: Insights from Guild Meeting"
categories: [microservices, it-security]
tags: [guild, individual, reflection]
lang: en
locale: en
nav_order: 16
ref: log-2025-11-06-guild-meeting-insights
---

##### Microservices and Kubernetes

It was emphasized that the exam focuses on what I have learned about microservices and Kubernetes, both theoretically and in practice. The project is only evidence of this learning, not a goal in itself. Therefore, I should explain my own architectural decisions, my implementations, and how I acquired the understanding, for example through videos and experiments.

Rather than listing which microservices the project contains, it is more meaningful to explain how microservices work in theory and how I implemented them myself in practice. It was made clear that the focus should be on my own architectural decisions, my implementations, and my understanding of the principles, not a detailed walkthrough of others’ code or services. Likewise, the other services do not need to be deployed in Kubernetes.

##### IT Security

**CIA model as a framework**  
Thomas recommended structuring IT security using the CIA triad: Confidentiality, Integrity, and Availability. The model is used to evaluate where security efforts make the most sense in relation to the data the system handles. A perfectly secure system is impossible, and security is therefore a matter of balance and prioritization.

**Web and API Security**  
We discussed why web security and API security are closely related: both are built on the HTTP protocol (typically port 80/443) and share many attack surfaces and countermeasures. The internet is the underlying infrastructure, while “web” refers to the part of the traffic that uses HTTP.

**Database Security**  
Database security ties into the same three pillars:

- **Confidentiality:** Access control, least privileges, encryption of data at rest and in transit  
- **Integrity:** Logging, audit trails, and mechanisms ensuring data cannot be changed unnoticed  
- **Availability:** Firewall configuration, hidden ports, brute-force protection, backups, and recovery procedures  

We evaluated which parts of the CIA triad different mechanisms support:

- Access control and authentication → **Confidentiality**  
- Regular security updates → primarily **Confidentiality**, but can support all three  
- Network security and firewalls → **Availability** and **Integrity**  
- Monitoring and logging → **Integrity**  

**Web Security**  
We looked at how the same principles apply to web applications. Threats such as XSS and SQL injection exploit HTTP as the entry point.

Again, evaluated through the CIA triad:

- Who may access the API? → **Confidentiality**  
- Can we trust the data we receive? → **Integrity**  
- How do we ensure uptime? Do we need a load balancer or a recovery plan? → **Availability**  

**Summary**  
When evaluating system security, the starting point is the domain and documentation, followed by an assessment of where the risks lie and which measures make practical sense. We cannot include every technology, so it's important to choose carefully.

##### Preparation for upcoming guild meetings  
**Microservices and Kubernetes:** We will discuss how to present our project and our technical points for the exam.  
**IT Security:** If we want feedback on learning objectives, these can be sent a few days before the next guild meeting.