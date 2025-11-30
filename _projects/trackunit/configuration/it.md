---
title: "Trackunit: Configurazione"
categories: [trackunit]
tags: [configuration, infrastructure, final]
lang: it
locale: it
nav_order: 23
ref: configuration-graphql-local
---

Aggiungi una voce DNS locale per `graphql.local`:
1. Apri il file hosts:
   ```bash
   sudo vim /etc/hosts
   ```
2. Aggiungi la voce host:
   ```bash
   172.21.0.4  graphql.local
   ```
   Sostituisci `172.21.0.4` con l’effettivo `EXTERNAL-IP` di `kong-kong-proxy`, disponibile dopo la prima esecuzione del cluster Kubernetes locale.
3. Salva il file.