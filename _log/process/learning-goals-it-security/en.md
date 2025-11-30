---
title: "Individual Learning Goals: IT Security (15 ECTS)"
categories: [it-security, microservices, kubernetes]
tags: [individual]
lang: en
locale: en
nav_order: 22
ref: learning-goals-it-security
---

**Knowledge**

I have:  
- An understanding of east–west traffic in Kubernetes and how service meshes provide mTLS, service identity and security policies between services.  
- Knowledge of the processes behind token issuance and validation in OAuth 2.0 and OIDC, including the use of Microsoft Entra as an Identity Provider.  
- Knowledge of secure handling of secrets, including principles for storing them in Kubernetes Secrets and options for external secret storage.  
- Knowledge of logging and tracing as security tools, including their importance for data integrity and detection of unauthorized activity.  
- An understanding of shared responsibility in cloud-native environments and how security and operational responsibility is distributed between cloud provider and developer.  
- Knowledge of risk assessment and methods for prioritising security measures.

**Skills**

I can:  
- Set up an API gateway solution using Kong to handle incoming traffic with security features such as TLS, rate limiting and OIDC-based authentication through oauth2-proxy.  
- Implement zero-trust principles by ensuring token validation in both oauth2-proxy and graph-gateway as part of the overall access control flow.  
- Configure TLS communication with self-signed certificates between clients and Kubernetes load balancers to secure traffic outside the cluster.

**Competences**

I can:  
- Design secure network policies for both Kubernetes deployments and Helm-based applications.  
- Build a coherent security architecture that only allows authorised clients access via JWT/OIDC-based authentication and rejects unauthorised requests at the API gateway.  
- Evaluate and prioritise relevant security measures in microservices- and Kubernetes-based systems.