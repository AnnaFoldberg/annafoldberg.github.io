---
title: "Trackunit: Kubernetes Namespace – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: en
locale: en
nav_order: 35
ref: graph-gateway-namespace
---

This manifest creates a dedicated namespace for **graph-gateway** and sets the `kubernetes.io/metadata.name` label, which is used by network policies to match the namespace.

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: graph-gateway
  labels:
    kubernetes.io/metadata.name: graph-gateway
```