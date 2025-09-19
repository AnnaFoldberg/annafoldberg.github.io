---
title: "Access Scenarios"
categories: [microservices, it-security]
tags: [access-control, api-gateway]
lang: en
locale: en
nav_order: 20
ref: note-access-scenarios
---
Access to a microservice occurs through a client. In most systems, client calls are routed through the API gateway, which centralizes access and control.

Clients differ in how they handle credentials. **Public clients**, such as single-page applications running in a browser, cannot keep credentials confidential. **Confidential clients**, like server-side applications, can protect them to a greater degree.

Ownership also matters. **First-party clients** belong to the same organization that built the service, while **third-party clients** are external and require stricter security controls since their posture cannot be guaranteed. Some organizations apply these stricter standards to both.

Finally, clients can be **internal** or **external**. Internal applications run inside the corporate firewall, while external ones face the public internet. However, network position should never reduce security requirements, as insider threats and lateral movement remain possible once the perimeter is breached.

<small> Source: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>