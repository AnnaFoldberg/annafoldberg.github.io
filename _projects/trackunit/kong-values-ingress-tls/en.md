---
title: "Trackunit: Kong values.ingress-tls.yaml"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: en
locale: en
nav_order: 36
ref: kong-values-ingress-tls
---

`values.ingress-tls.yaml` is used by the **Kong Helm chart** to configure **Kong** inside the cluster. It enables the **Kong Ingress Controller** with the key `ingressClass: kong`, and configures the **Kong Proxy** as a LoadBalancer service with TLS on port `443`. The configuration specifies that Kong runs in dbless mode (`database: off`), and mounts the local development certificate `kong-ingress-tls`, allowing the proxy layer to terminate HTTPS traffic from the client.

```yaml
ingressController:
  enabled: true
  ingressClass: kong
env:
  database: "off"
proxy:
  type: LoadBalancer
  http:
    enabled: false
  tls:
    enabled: true
    servicePort: 443
    containerPort: 8443
secretVolumes:
  - kong-ingress-tls
```