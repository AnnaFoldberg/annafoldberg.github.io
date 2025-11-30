---
title: "livenessProbe vs readinessProbe"
categories: [kubernetes]
tags: [probes, healthchecks, liveness, readiness]
lang: en
locale: en
nav_order: 64
ref: note-liveness-vs-readiness
---

##### readinessProbe

A readinessProbe answers whether the container can receive traffic now.

If the readiness probe fails, the Pod is removed from the Service endpoints, which means it will no longer receive network traffic from Services. The container is not restarted, it simply will not be used until it reports ready again.

##### livenessProbe

A livenessProbe answers whether the container is stuck or in a state where it cannot recover. If the liveness probe fails, Kubernetes considers the container to be stalled and automatically restarts it.