---
title: "Trackunit: System Network Communication Architecture"
categories: [trackunit, microservices]
tags: [architecture, network, communication, final]
lang: da
locale: da
nav_order: 51
ref: system-network-communication-architecture
---

![system-network-communication-architecture.png](../../../assets/images/projects/trackunit/system-network-communication-architecture/system-network-communication-architecture.png)

> **Tip:** Åbn diagrammet i en ny fane for at zoome ind og se detaljer.

Diagrammet giver et samlet overblik over systemets netværksflow. Det viser forbindelsen fra **trackunit-client** gennem **kong-kong-proxy**, trafikken mellem interne services, netværkspolitikkernes ingress- og egress-regler samt TLS-terminering mellem **trackunit-client** og **kong-kong-proxy**.

![client-kong-oauth2-proxy.png](../../../assets/images/projects/trackunit/system-network-communication-architecture/client-kong-oauth2-proxy.png)

Dette udsnit viser forbindelsen fra **trackunit-client** ind i clusteret gennem **kong-kong-proxy**, inkl. TLS-terminering, routing videre til **oauth2-proxy** og derefter til **graph-gateway**. Det illustrerer også de ingress- og egress-regler og services, som styrer dataflowet frem til **graph-gateway**.

![graph-gateway.png](../../../assets/images/projects/trackunit/system-network-communication-architecture/graph-gateway.png)

Dette udsnit viser **graph-gateway’s** interne kommunikation i clusteret, herunder hvordan dens pods modtager trafik fra **oauth2-proxy** og forbinder til **rabbitmq**. Det viser også **graph-gateway’s** eksterne kald til **Microsoft Entra ID** (OIDC discovery og JWKS endpoints) og til **tu-ingestion-service** via en **tailscale funnel** adresse. Derudover fremgår dets **namespace**, **ClusterIP service** og **network policy** med tilhørende ingress- og egress-regler.

![svc-analysis-orchestrator.png](../../../assets/images/projects/trackunit/system-network-communication-architecture/svc-analysis-orchestrator.png)

Dette udsnit viser, hvordan **svc-analysis-orchestrator** forbinder til **rabbitmq**. Det illustrerer også dets **namespace**, **ClusterIP service** og **network policy** med tilhørende ingress- og egress-regler.

Det viser desuden, hvordan **svc-messaging-bridge** tilgår Kafka-brokeren **redpanda**, samt clusterets **rabbitmq**-pod (via port-forward til **rabbitmq** ClusterIP-service).