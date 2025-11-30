---
title: "livenessProbe vs readinessProbe"
categories: [kubernetes]
tags: [probes, healthchecks, liveness, readiness]
lang: it
locale: it
nav_order: 64
ref: note-liveness-vs-readiness
---

##### readinessProbe

Una readinessProbe indica se il container può ricevere traffico ora.

Se la readiness probe fallisce, il Pod viene rimosso dagli endpoints del Service, il che significa che non riceverà più traffico di rete dai Services. Il container non viene riavviato, semplicemente non verrà utilizzato finché non segnala nuovamente di essere pronto.

##### livenessProbe

Una livenessProbe indica se il container è bloccato o in uno stato da cui non può riprendersi. Se la liveness probe fallisce, Kubernetes considera il container come bloccato e lo riavvia automaticamente.