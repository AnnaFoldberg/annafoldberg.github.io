---
title: "Trackunit: Kubernetes Service – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: it
locale: it
nav_order: 33
ref: graph-gateway-service
---

Questo manifest definisce un servizio ClusterIP per **graph-gateway**. Il servizio seleziona i pod con la stessa `label-selector` ed espone la porta `8080` internamente al cluster, permettendo ad altri servizi di accedere a **graph-gateway** tramite un nome DNS stabile invece di indirizzi pod diretti.

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