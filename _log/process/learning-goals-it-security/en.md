---
title: "Individual Learning Goals: IT Security (15 ECTS)"
categories: [it-security, microservices, kubernetes]
tags: [individual]
lang: en
locale: en
nav_order: 22
ref: learning-goals-it-security
---

##### Knowledge  
I have:  
- Understanding of east–west traffic and how service meshes in Kubernetes handle mTLS encryption, service identity, and security policies across services.  
- Knowledge of the process behind issuing and validating tokens via OAuth 2.0 and OIDC, including the use of Microsoft Entra as the Identity Provider.  
- Knowledge of secure handling of secrets, including principles for storing them in Kubernetes Secrets as well as awareness of external secret storage.  
- Knowledge of logging and tracing as security tools, including their role in ensuring data integrity and detecting unauthorized activity.  
- Understanding of shared responsibility in cloud-native environments, and how responsibility for security and operations is divided between cloud provider and developer.  
- Understanding of how security measures are prioritized based on system size, threat model, and architectural complexity.  

##### Skills  
I can:  
- Set up an API gateway solution based on Kong for handling inbound traffic with security features such as TLS and rate limiting, as well as integration with OIDC-based authentication via oauth2-proxy.  
- Implement zero-trust principles by ensuring token validation in both oauth2-proxy and graph-gateway as part of the overall access control flow.  
- Set up and use Kubernetes Secrets via separate secret manifests for use in both Kubernetes deployments and Helm releases.  
- Set up TLS termination with self-signed certificates via an Ingress Controller and Ingress resource to ensure encrypted traffic between the client and the system’s entry point.  

##### Competencies  
I can:  
- Evaluate relevant security measures in microservices- and Kubernetes-driven systems.  
- Develop secure network policies for both Kubernetes deployments and Helm releases.  
- Design a security architecture that combines encrypted traffic between the client and the system’s entry point with authentication and rate limiting at the API gateway, ensuring that unauthorized and abusive traffic is blocked before reaching internal services.