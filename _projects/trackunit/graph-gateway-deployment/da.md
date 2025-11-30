---
title: "Trackunit: Kubernetes Deployment – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, deployment, final]
lang: da
locale: da
nav_order: 31
ref: graph-gateway-deployment
---

Dette manifest viser den konkrete opsætning for **graph-gateway** i clusteret. Deploymentet specificerer `3` replicas, indlæser environment variables fra `graph-gateway-secret`, definerer health probes og bruger en `RollingUpdate`-strategi.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: graph-gateway
  namespace: graph-gateway
  labels:
    app.kubernetes.io/name: graph-gateway
spec:
  replicas: 3
  selector:
    matchLabels:
      app.kubernetes.io/name: graph-gateway
  template:
    metadata:
      labels:
        app.kubernetes.io/name: graph-gateway
    spec:
      terminationGracePeriodSeconds: 20
      containers:
      - name: graph-gateway
        image: ghcr.io/team-2-devs/graph-gateway:24
        ports:
        - containerPort: 8080
        envFrom:
        - secretRef:
            name: graph-gateway-secret
        readinessProbe:
          httpGet:
            path: /health/ready
            port: 8080
          periodSeconds: 5
          timeoutSeconds: 3
          failureThreshold: 30
        livenessProbe:
          httpGet:
            path: /health/live
            port: 8080
          periodSeconds: 5
          timeoutSeconds: 3
          failureThreshold: 30
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
```