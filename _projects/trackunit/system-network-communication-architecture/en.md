---
title: "Trackunit: System Network Communication Architecture"
categories: [trackunit, microservices]
tags: [architecture, network, communication, final]
lang: en
locale: en
nav_order: 51
ref: system-network-communication-architecture
---

![system-network-communication-architecture.png](../../../assets/images/projects/trackunit/system-network-communication-architecture/system-network-communication-architecture.png)

> **Tip:** Open the diagram in a new tab to zoom in and see details.

The diagram provides a complete overview of the system’s network flow. It shows the connection from **trackunit-client** through **kong-kong-proxy**, the traffic between internal services, the ingress and egress rules enforced by network policies, and the TLS termination between **trackunit-client** and **kong-kong-proxy**.

![client-kong-oauth2-proxy.png](../../../assets/images/projects/trackunit/system-network-communication-architecture/client-kong-oauth2-proxy.png)

This section shows the connection from **trackunit-client** into the cluster through **kong-kong-proxy**, including TLS termination, routing to **oauth2-proxy**, and then to **graph-gateway**. It also illustrates the ingress and egress rules and the services that govern the data flow toward **graph-gateway**.

![graph-gateway.png](../../../assets/images/projects/trackunit/system-network-communication-architecture/graph-gateway.png)

This section shows **graph-gateway’s** internal communication in the cluster, including how its pods receive traffic from **oauth2-proxy** and connect to **rabbitmq**. It also highlights **graph-gateway’s** external calls to **Microsoft Entra ID** (OIDC discovery and JWKS endpoints) and to **tu-ingestion-service** via a **tailscale funnel** address. Additionally, it shows its **namespace**, **ClusterIP service**, and **network policy** with associated ingress and egress rules.

![svc-analysis-orchestrator.png](../../../assets/images/projects/trackunit/system-network-communication-architecture/svc-analysis-orchestrator.png)

This section shows how **svc-analysis-orchestrator** connects to **rabbitmq**. It also illustrates its **namespace**, **ClusterIP service**, and **network policy** with associated ingress and egress rules.

It also shows how **svc-messaging-bridge** accesses the Kafka broker **redpanda**, as well as the cluster’s **rabbitmq** pod (through port-forwarding to the **rabbitmq** ClusterIP service).