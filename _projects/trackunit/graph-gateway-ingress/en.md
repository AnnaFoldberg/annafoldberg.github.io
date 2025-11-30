---
title: "Trackunit: Kubernetes Ingress – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: en
locale: en
nav_order: 37
ref: graph-gateway-ingress
---

This manifest defines the Ingress resource that the **Kong Ingress Controller** translates into a route inside **Kong**. It specifies that HTTPS traffic to the host `graphql.local` and path `/graphql` must be terminated with TLS using `kong-ingress-tls` and forwarded to **oauth2-proxy** on port `4180`. The key `ingressClassName: kong` ensures that the resource is handled by the **Kong Ingress Controller**.

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