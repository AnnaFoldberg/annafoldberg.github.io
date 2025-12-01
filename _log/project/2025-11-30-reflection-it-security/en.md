---
title: "2025-11-30: The Role of IT Security in the Project"
categories: [it-security, kubernetes, microservices, trackunit]
tags: [reflection, project, individual]
lang: en
locale: en
nav_order: 19
ref: log-2025-11-30-reflection-it-security
---

##### Security Measures Covered in the Project

The project made it possible to work with several core security mechanisms in a cloud-native setup. The most important part was access control, where I implemented rate-limiting in Kong and OIDC-based authentication via oauth2-proxy, followed by token validation in graph-gateway. This ensured that only clients with valid JWT tokens could call the API gateway, and that unauthorized requests or misuse traffic were blocked early in the flow.

I also worked with transport-layer security by setting up TLS termination through an Ingress Controller with self-signed certificates, ensuring that traffic between the client and the system’s entry point was encrypted.

Additionally, I configured Network Policies to restrict traffic between pods and provide fine-grained control over which services are allowed to communicate. This reduced the attack surface inside the cluster by permitting only necessary traffic and blocking everything else by default.

Finally, I used Kubernetes Secrets to handle sensitive information in a more secure way, so credentials were not placed directly inside deployments or values files. This provided a cleaner separation between configuration and secrets and a more responsible approach to handling sensitive data.

##### Security Measures Not Covered in the Project

There are also security aspects that the project intentionally does not include. For example, I did not use a service mesh for mTLS between services. Although it provides stronger identity handling and encryption inside the cluster, it would have added substantial complexity that did not match the project’s scope or learning goals.

I also did not work with centralized logging and tracing. In a production environment these are important for detecting suspicious activity and investigating issues, but here I chose to focus on the core mechanisms such as access control, TLS, Secrets, and network restrictions.

##### Summary

The project covers the most relevant security elements for a system of this size, including client-to-cluster encryption, authentication, rate-limiting, token validation, Secrets handling, and network traffic restrictions. More advanced technologies such as service mesh and full observability were deliberately left out to keep the focus on the areas that offer the most learning value without adding unnecessary complexity.