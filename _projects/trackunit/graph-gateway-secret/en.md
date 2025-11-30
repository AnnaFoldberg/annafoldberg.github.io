---
title: "Trackunit: Kubernetes Secret – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: en
locale: en
nav_order: 32
ref: graph-gateway-secret
---

This manifest defines a Kubernetes Secret containing environment variables for **graph-gateway**. It resides in the `graph-gateway` namespace and is exposed to the deployment via `envFrom.secretRef`. It uses `stringData` so values can be written in cleartext and automatically converted to base64 by Kubernetes.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: graph-gateway-secret
  namespace: graph-gateway
stringData:
  ...
```