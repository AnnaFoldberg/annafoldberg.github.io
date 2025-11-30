---
title: "Trackunit: Kubernetes Service – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: da
locale: da
nav_order: 33
ref: graph-gateway-service
---

Dette manifest definerer en ClusterIP service for **graph-gateway**. Servicen matcher pods med samme `label-selector` og eksponerer port `8080` internt i clusteret, så andre services kan tilgå **graph-gateway** via et stabilt DNS-navn i stedet for direkte pod-adresser.

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