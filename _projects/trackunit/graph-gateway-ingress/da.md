---
title: "Trackunit: Kubernetes Ingress – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: da
locale: da
nav_order: 37
ref: graph-gateway-ingress
---

Dette manifest definerer den Ingress-ressource, som **Kong Ingress Controller** omsætter til en route i **Kong**. Den angiver, at HTTPS-trafik til hosten `graphql.local` og path `/graphql` skal termineres med TLS via `kong-ingress-tls` og sendes videre til **oauth2-proxy** på port `4180`. `ingressClassName: kong` sikrer, at ressourcen håndteres af **Kong Ingress Controlleren**.

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