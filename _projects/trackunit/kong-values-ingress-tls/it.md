---
title: "Trackunit: Kong values.ingress-tls.yaml"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: it
locale: it
nav_order: 36
ref: kong-values-ingress-tls
---

`values.ingress-tls.yaml` viene utilizzato dal **Kong Helm chart** per configurare **Kong** nel cluster. Qui viene abilitato il **Kong Ingress Controller** con la chiave `ingressClass: kong` e il **Kong Proxy** viene configurato come servizio LoadBalancer con TLS sulla porta `443`. La configurazione specifica che Kong viene eseguito in modalità dbless (`database: off`) e monta il certificato di sviluppo locale `kong-ingress-tls`, consentendo al livello proxy di terminare il traffico HTTPS proveniente dal client.

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