---
title: "Trackunit: Choice of Replicas"
categories: [trackunit, kubernetes]
tags: [pods, replicas, scaling, final]
lang: en
locale: en
nav_order: 46
ref: pod-replicas
---

##### Own services

- **graph-gateway:** Runs with `3` pods because it is the system’s entry point. Three replicas provide higher availability in case of pod crashes and better capacity to handle traffic spikes.  
- **svc-analysis-orchestrator:** Runs with `2` pods because it is central to the analysis flow but not directly on the client’s request path. Two replicas provide fault tolerance without using as many resources as the gateway.

##### Helm-deployed components

- **Kong**, **oauth2-proxy**, and **RabbitMQ:** Each runs with `1` pod. They are infrastructure services and change less frequently compared to the project’s own services. More replicas would increase complexity without adding significant value in the current setup. However, they are still single points of failure, since large parts of the system are affected if any of them becomes unavailable.

##### Summary

In the Trackunit project, `1` pod per service would in practice be sufficient, because the system is small, runs locally, and only serves a single client. Most components are mutually dependent, so a failure in one service will inevitably impact the whole.

The number of replicas for **graph-gateway** and **svc-analysis-orchestrator** is therefore mainly chosen to demonstrate how services can be scaled according to their role and criticality in a more production-like environment. The scaling choices show how availability and robustness can be increased where it makes the most sense, while keeping the infrastructure simple.