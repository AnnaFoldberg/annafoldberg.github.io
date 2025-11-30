---
title: "Trackunit: System Network Communication Architecture"
categories: [trackunit, microservices]
tags: [architecture, network, communication, final]
lang: it
locale: it
nav_order: 51
ref: system-network-communication-architecture
---

![system-network-communication-architecture.png](../../../assets/images/projects/trackunit/system-network-communication-architecture/system-network-communication-architecture.png)

> **Suggerimento:** Apri il diagramma in una nuova scheda per ingrandire e vedere i dettagli.

Il diagramma fornisce una panoramica completa del flusso di rete del sistema. Mostra la connessione da **trackunit-client** attraverso **kong-kong-proxy**, il traffico tra i servizi interni, le regole di ingress ed egress definite dalle network policy e la terminazione TLS tra **trackunit-client** e **kong-kong-proxy**.

![client-kong-oauth2-proxy.png](../../../assets/images/projects/trackunit/system-network-communication-architecture/client-kong-oauth2-proxy.png)

Questa sezione mostra la connessione da **trackunit-client** verso l’interno del cluster tramite **kong-kong-proxy**, includendo la terminazione TLS, il routing verso **oauth2-proxy** e successivamente verso **graph-gateway**. Illustra inoltre le regole di ingress ed egress e i servizi che regolano il flusso di dati verso **graph-gateway**.

![graph-gateway.png](../../../assets/images/projects/trackunit/system-network-communication-architecture/graph-gateway.png)

Questa sezione mostra la comunicazione interna di **graph-gateway** nel cluster, compreso come i suoi pod ricevano traffico da **oauth2-proxy** e si connettano a **rabbitmq**. Evidenzia inoltre le chiamate esterne di **graph-gateway** verso **Microsoft Entra ID** (OIDC discovery e JWKS endpoints) e verso **tu-ingestion-service** tramite un indirizzo **tailscale funnel**. Vengono mostrati anche il relativo **namespace**, il **servizio ClusterIP** e la **network policy** con le rispettive regole di ingress ed egress.

![svc-analysis-orchestrator.png](../../../assets/images/projects/trackunit/system-network-communication-architecture/svc-analysis-orchestrator.png)

Questa sezione mostra come **svc-analysis-orchestrator** si connetta a **rabbitmq**. Illustra inoltre il suo **namespace**, il **servizio ClusterIP** e la **network policy** con le relative regole di ingress ed egress.

Mostra anche come **svc-messaging-bridge** acceda al broker Kafka **redpanda**, così come il pod **rabbitmq** del cluster (tramite port-forward verso il servizio ClusterIP di **rabbitmq**).