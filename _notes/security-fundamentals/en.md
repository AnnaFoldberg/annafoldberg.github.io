---
title: "Security Fundamentals"
categories: [microservices, it-security]
tags: [cia-triad, access-control, attack-surface]
lang: en
locale: en
nav_order: 16
ref: note-security-fundamentals
---
**Confidentiality:** Controlling access to data to prevent unauthorized disclosure  

**Integrity:** Ensuring data is not tampered with and can be trusted  

**Availability:** Authorized users may access data when needed  

To achieve these goals, security strategies and controls are selected and applied to a system’s architecture to the extent they are required.

![CIA triad](../../../assets/images/notes/security-fundamentals/cia-triad.png)

**Access control**

1. Authentication: Establish identity  
2. Authorization: Verify access privileges  
3. Resource access  

**Authentication**

- Identity verification for parties accessing a system  
- Human or non-human (system)  
- Credentials or tokens prove identity  
- Highly sensitive systems may leverage multi-factor authentication (MFA)  

**Authorization**

- Ensures authorized access and use of resources  
- Identification and enforcement of privileges  
- May be delegated to third parties  

**Trust**

- Determines to what extent something is believed to be true.  

**Attack surface**

Comprises all of the paths that can be used to get data into or out of an application. A system’s user interface, open ports, API, and database can all present opportunities for an attacker to compromise a system. So they are considered part of the attack surface. Reducing and hardening the attack surface is an effective strategy to enhance a system’s security.

<small> Source: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>