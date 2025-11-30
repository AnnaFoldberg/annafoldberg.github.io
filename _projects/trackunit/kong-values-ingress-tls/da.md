---
title: "Trackunit: Kong values.ingress-tls.yaml"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: da
locale: da
nav_order: 36
ref: kong-values-ingress-tls
---

`values.ingress-tls.yaml` bruges af **Kong Helm-chartet** til at konfigurere **Kong** i clusteret. Her aktiveres **Kong Ingress Controller** med nøglen `ingressClass: kong`, og **Kong Proxy** konfigureres som en LoadBalancer-service med TLS på port `443`. Konfigurationen angiver, at Kong kører i dbless-tilstand (`database: off`), og at det lokale udviklingscertifikat `kong-ingress-tls` monteres, så proxylaget kan terminere HTTPS-trafik fra klienten.

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