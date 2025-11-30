---
title: "Trackunit: Kubernetes NetworkPolicy – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, network-policy, final]
lang: da
locale: da
nav_order: 34
ref: graph-gateway-network-policy
---

Dette manifest definerer en NetworkPolicy for **graph-gateway**. Den begrænser hvilke pods, der må sende trafik til **graph-gateway** (`ingress`), og hvilke eksterne og interne endpoints **graph-gateway** selv må kontakte (`egress`). Policyen tillader kun indgående trafik fra **oauth2-proxy** i `api-gateway`-namespace, og udgående trafik til **rabbitmq**, **CoreDNS**, samt udgående HTTPS-trafik (`port: 443`) til Microsoft Entra ID og Tailscale funnel-adressen.

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