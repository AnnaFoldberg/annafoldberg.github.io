---
title: "Trackunit: Kubernetes Namespace – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: da
locale: da
nav_order: 35
ref: graph-gateway-namespace
---

Dette manifest opretter et dedikeret namespace til **graph-gateway** og sætter `kubernetes.io/metadata.name`-label, som bruges af network policies til at matche namespacet.

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: graph-gateway
  labels:
    kubernetes.io/metadata.name: graph-gateway
```