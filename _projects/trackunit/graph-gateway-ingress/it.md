---
title: "Trackunit: Ingress Kubernetes – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: it
locale: it
nav_order: 37
ref: graph-gateway-ingress
---

Questo manifest definisce la risorsa Ingress che il **Kong Ingress Controller** traduce in una route all’interno di **Kong**. Specifica che il traffico HTTPS verso l’host `graphql.local` e il path `/graphql` deve essere terminato tramite TLS usando `kong-ingress-tls` e inoltrato a **oauth2-proxy** sulla porta `4180`. La chiave `ingressClassName: kong` garantisce che la risorsa venga gestita dal **Kong Ingress Controller**.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-graphql
  namespace: api-gateway
  annotations:
    konghq.com/strip-path: "false"
    konghq.com/protocols: "https" # restricts route to HTTPS only
spec:
  ingressClassName: kong
  tls:
  - hosts:
      - graphql.local
    secretName: kong-ingress-tls
  rules:
  - host: graphql.local
    http:
      paths:
      - path: /graphql
        pathType: Prefix
        backend:
          service:
            name: oauth2-proxy
            port:
              number: 4180
```