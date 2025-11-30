---
title: "Trackunit: Kubernetes Secret – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: it
locale: it
nav_order: 32
ref: graph-gateway-secret
---

Questo manifest definisce un Secret di Kubernetes che contiene variabili d’ambiente per **graph-gateway**. Si trova nel namespace `graph-gateway` ed è esposto al deployment tramite `envFrom.secretRef`. Usa `stringData` affinché i valori possano essere scritti in chiaro e convertiti automaticamente in base64 da Kubernetes.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: graph-gateway-secret
  namespace: graph-gateway
stringData:
  ...
```