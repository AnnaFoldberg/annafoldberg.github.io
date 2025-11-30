---
title: "Trackunit: Kong come Ingress Controller e API Gateway - api-gateway"
categories: [trackunit, kubernetes]
tags: [kong, ingress, api-gateway, configuration, final]
lang: it
locale: it
nav_order: 45
ref: kong-ingress-vs-proxy
---

##### Ingress Controller

Il **Kong Ingress Controller** viene eseguito come container separato nello stesso Deployment del **Kong Proxy** e legge risorse Kubernetes come `Ingress`.  
Sulla base di queste risorse crea la configurazione necessaria dentro **Kong** tramite il Kong Admin API.  
L’Ingress Controller *non* processa traffico; si occupa esclusivamente di configurare la gateway.

`IngressClass: kong` viene creata automaticamente dall’Helm chart quando `ingressController.enabled: true`.

```yaml
# values.ingress-tls.yaml

ingressController:
  enabled: true
  ingressClass: kong
...
```

##### Kong Proxy

Il **Kong Proxy** è la gateway vera e propria e gestisce tutto il traffico in ingresso.  
Ascolta sulla porta `443` (tramite il servizio LoadBalancer) e abbina host e path sulla base della configurazione generata dal Kong Ingress Controller da `ingress-graphql.yaml`.

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

##### Risorsa Ingress

La risorsa Ingress definisce host, path e impostazioni TLS e fa riferimento a `IngressClass: kong`.  
È solo una dichiarazione: sarà il **Kong Ingress Controller** a convertirla nella configurazione effettiva dentro **Kong**.

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

##### TLS (certificato self-signed)

Per lo sviluppo locale viene utilizzato un certificato self-signed, `kong-ingress-tls`, memorizzato come Secret Kubernetes e referenziato dalla risorsa Ingress.  
L’Ingress Controller registra il certificato e crea la relativa configurazione TLS in **Kong**, dopodiché il **Kong Proxy** presenta il certificato a runtime.