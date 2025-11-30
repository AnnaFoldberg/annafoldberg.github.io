---
title: "Trackunit: Configuration"
categories: [trackunit]
tags: [configuration, infrastructure, final]
lang: da
locale: da
nav_order: 23
ref: configuration-graphql-local
---

Tilføj lokal DNS-entry for `graphql.local`:
1. Åbn hosts-filen:
   ```bash
   sudo vim /etc/hosts
   ```
2. Tilføj host-entry:
   ```bash
   172.21.0.4  graphql.local
   ```
   Erstat `172.21.0.4` med den faktiske `EXTERNAL-IP` for `kong-kong-proxy`, som er tilgængelig efter første kørsel af det lokale Kubernetes-cluster.
3. Gem filen.