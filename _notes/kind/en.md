---
title: "Kind"
categories: [kubernetes]
tags: [kind, clusters]
lang: en
locale: en
nav_order: 55
ref: note-kind
---

Kind is a tool that makes it possible to run Kubernetes clusters locally by using Docker containers as nodes. It was originally designed for testing Kubernetes, but it can be used for local development, testing, and proof-of-concepts where a real Kubernetes environment is needed without relying on a cloud provider.

Kind creates a complete cluster inside Docker, where each node in the cluster corresponds to a separate container.

##### External IPs in Kind

Because Kind does not run in a cloud environment, external load balancers are not created automatically. To use the LoadBalancer Service type in Kind, an additional component must be added that simulates a cloud provider. This makes it possible to test components such as API gateways, which normally rely on an external load balancer.

Alternatively, the NodePort Service type can be used for local development.

##### Kind commands

- Create cluster: `kind create cluster`  
- Delete cluster: `kind delete cluster`  
- Simulate cloud provider: `sudo go/bin/cloud-provider-kind`

<small>Source: [kind Documentation](https://kind.sigs.k8s.io/)</small>