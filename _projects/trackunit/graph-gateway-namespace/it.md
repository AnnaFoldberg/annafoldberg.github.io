---
title: "Trackunit: Namespace Kubernetes – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: it
locale: it
nav_order: 35
ref: graph-gateway-namespace
---

Questo manifest crea un namespace dedicato per **graph-gateway** e imposta l’etichetta `kubernetes.io/metadata.name`, utilizzata dalle network policy per individuare correttamente il namespace.

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: graph-gateway
  labels:
    kubernetes.io/metadata.name: graph-gateway
```