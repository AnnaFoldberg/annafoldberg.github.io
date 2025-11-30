---
title: "Trackunit: Kong som Ingress Controller og API-gateway"
categories: [trackunit, kubernetes]
tags: [kong, ingress, api-gateway, configuration, final]
lang: da
locale: da
nav_order: 45
ref: kong-ingress-vs-proxy
---

##### Ingress Controller

**Kong Ingress Controller** kører som en separat container i det samme Deployment som **Kong Proxy** og læser Kubernetes-ressourcer som `Ingress`.  
På baggrund af disse ressourcer oprettes den nødvendige konfiguration i **Kong** via Kong Admin API.  
Ingress Controlleren behandler ikke trafik, men står udelukkende for at konfigurere gatewayen.  

`IngressClass: kong` oprettes automatisk af Helm-chartet, når `ingressController.enabled: true`.

```yaml
# values.ingress-tls.yaml

ingressController:
  enabled: true
  ingressClass: kong
...
```

##### Kong Proxy

**Kong Proxy** er selve gatewayen og håndterer al indgående trafik.  
Den lytter på port `443` (via LoadBalancer-servicen) og matcher host og sti ud fra den konfiguration, som Kong Ingress Controller har genereret på baggrund af `ingress-graphql.yaml`.

```yaml
# values.ingress-tls.yaml

...
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

##### Ingress-ressourcen

Ingress-ressourcen definerer host, path og TLS-indstillinger og refererer `IngressClass: kong`.  
Ressourcen er udelukkende en deklaration, som omsættes til reel konfiguration i **Kong** af Ingress Controlleren.

```yaml
# ingress-graphql.yaml

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-graphql
  namespace: api-gateway
  annotations:
    konghq.com/strip-path: "false"
    konghq.com/protocols: "https"
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

##### TLS (self-signed certifikat)

Til lokal udvikling anvendes et self-signed certifikat, `kong-ingress-tls`, der ligger som et Kubernetes Secret og refereres fra Ingress-ressourcen.  
Ingress Controlleren registrerer certifikatet og opretter den tilsvarende TLS-opsætning i **Kong**, hvorefter **Kong Proxy** præsenterer certifikatet ved runtime.