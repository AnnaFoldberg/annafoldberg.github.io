---
title: "livenessProbe vs readinessProbe"
categories: [kubernetes]
tags: [probes, healthchecks, liveness, readiness]
lang: da
locale: da
nav_order: 64
ref: note-liveness-vs-readiness
---

##### readinessProbe

En readinessProbe svarer på om containeren kan modtage trafik nu.

Hvis readiness-proben fejler, fjernes Pod’en fra Service-endpoints, hvilket betyder, at den ikke længere får netværkstrafik fra Services. Containeren genstartes ikke, den bliver bare ikke brugt, før den igen melder sig klar.

##### livenessProbe

En livenessProbe svarer på, om containeren er låst fast eller i en tilstand, hvor den ikke kan komme sig. Hvis liveness-proben fejler, vurderer Kubernetes, at containeren er gået i stå, og genstarter containeren automatisk.