---
title: "Trackunit: Kubernetes Secret – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: da
locale: da
nav_order: 32
ref: graph-gateway-secret
---

Dette manifest definerer et Kubernetes Secret, som indeholder environment variables til **graph-gateway**. Det ligger i `graph-gateway`-namespace og udstilles til deploymentet via `envFrom.secretRef`. Der bruges `stringData`, så værdierne kan angives i klartekst og automatisk konverteres til base64 af Kubernetes.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: graph-gateway-secret
  namespace: graph-gateway
stringData:
  ...
```