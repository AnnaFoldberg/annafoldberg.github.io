---
title: "Trackunit: Kubernetes NetworkPolicy – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, network-policy, final]
lang: it
locale: it
nav_order: 34
ref: graph-gateway-network-policy
---

Questo manifest definisce una NetworkPolicy per **graph-gateway**. Limita quali pod possono inviare traffico verso **graph-gateway** (`ingress`) e quali endpoint interni ed esterni **graph-gateway** può contattare (`egress`). La policy consente soltanto traffico in ingresso da **oauth2-proxy** nel namespace `api-gateway` e traffico in uscita verso **rabbitmq**, **CoreDNS**, oltre al traffico HTTPS (`port: 443`) verso Microsoft Entra ID e l’indirizzo del Tailscale funnel.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: graph-gateway
  namespace: graph-gateway
spec:
  podSelector:
    matchLabels:
      app.kubernetes.io/name: graph-gateway
  policyTypes:
  - Ingress
  - Egress
  ingress:
    - from:
      - namespaceSelector:
          matchLabels:
            kubernetes.io/metadata.name: api-gateway
        podSelector:
          matchLabels:
            app.kubernetes.io/name: oauth2-proxy
      ports:
        - protocol: TCP
          port: 8080
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: messaging
      podSelector:
        matchLabels:
          app.kubernetes.io/name: rabbitmq
    ports:
    - protocol: TCP
      port: 5672
  # Allow DNS to CoreDNS in kube-system
  - to:
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: kube-system
    ports:
    - protocol: TCP
      port: 53
    - protocol: UDP
      port: 53
  # External HTTPS (Microsoft Entra (OIDC/JWKS) + Tailscale funnel)
  - to:
    - ipBlock:
        cidr: 0.0.0.0/0
    ports:
      - protocol: TCP
        port: 443
```