---
title: "Trackunit: Kubernetes NetworkPolicy – graph-gateway"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, network-policy, final]
lang: en
locale: en
nav_order: 34
ref: graph-gateway-network-policy
---

This manifest defines a NetworkPolicy for **graph-gateway**. It restricts which pods may send traffic to **graph-gateway** (`ingress`), and which internal and external endpoints **graph-gateway** may contact (`egress`). The policy allows only incoming traffic from **oauth2-proxy** in the `api-gateway` namespace, and outgoing traffic to **rabbitmq**, **CoreDNS**, as well as outbound HTTPS traffic (`port: 443`) to Microsoft Entra ID and the Tailscale funnel address.

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