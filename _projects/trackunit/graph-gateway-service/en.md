---
title: "Trackunit: Kubernetes Service – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: en
locale: en
nav_order: 33
ref: graph-gateway-service
---

This manifest defines a ClusterIP service for **graph-gateway**. The service matches pods with the same `label selector` and exposes port `8080` internally within the cluster, allowing other services to access **graph-gateway** via a stable DNS name instead of direct pod addresses.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: graph-gateway
  namespace: graph-gateway
spec:
  selector:
    app.kubernetes.io/name: graph-gateway
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
```