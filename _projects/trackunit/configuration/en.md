---
title: "Trackunit: Configuration"
categories: [trackunit]
tags: [configuration, infrastructure, final]
lang: en
locale: en
nav_order: 23
ref: configuration-graphql-local
---

Add local DNS entry for `graphql.local`:
1. Open the hosts file:
   ```bash
   sudo vim /etc/hosts
   ```
2. Add host entry:
   ```bash
   172.21.0.4  graphql.local
   ```
   Replace `172.21.0.4` with the actual `EXTERNAL-IP` of `kong-kong-proxy`, available after running the local Kubernetes cluster for the first time.
3. Save the file.