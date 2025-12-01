---
title: "Ingress"
categories: [kubernetes]
tags: [ingress, tls, networking, routing]
lang: it
locale: it
nav_order: 65
ref: note-kubernetes-ingress
---

##### Ingress

Un Ingress è una risorsa Kubernetes che espone traffico HTTP e HTTPS dall’esterno del cluster verso i Services interni. Invece di aprire porte direttamente su un Service, un Ingress definisce regole di instradamento basate su percorsi o nomi di dominio.

L’oggetto Ingress richiede un Ingress Controller, che traduce le regole dell’Ingress in configurazioni effettive del load balancer.

##### TLS

Un Ingress può essere protetto con TLS facendo riferimento a un Secret che contiene un certificato e una chiave privata. L’Ingress gestisce il TLS solo sulla porta 443 e si aspetta che il traffico verso i Services avvenga senza crittografia-il TLS viene terminato a livello di Ingress.

La sezione TLS specifica quali host devono usare il certificato. Se vengono configurati più host, l’instradamento avviene tramite l’estensione SNI, a condizione che il controller la supporti.

Il certificato nel Secret deve chiamarsi `tls.crt` e `tls.key`, e il dominio (CN/FQDN) deve corrispondere all’host nella regola dell’Ingress.

>**SNI:** Il meccanismo che permette di ospitare più domini HTTPS sulla stessa IP, consentendo al client di rivelare il nome del dominio all’inizio dell’handshake TLS.

**Esempio di un semplice Ingress che inoltra traffico a un singolo Service con TLS**  

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: tls-example-ingress
spec:
  tls:
  - hosts:
      - https-example.foo.com
    secretName: testsecret-tls
  rules:
  - host: https-example.foo.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 80
```

##### Ingress Class

Più Ingress Controllers possono coesistere nello stesso cluster, quindi ogni Ingress dovrebbe specificare a quale classe appartiene. Una IngressClass definisce quale controller deve gestire gli oggetti Ingress che utilizzano tale classe e eventuali configurazioni aggiuntive.

Una IngressClass collega un oggetto Ingress alla specifica implementazione del controller.

```yaml
apiVersion: networking.k8s.io/v1
kind: IngressClass
metadata:
  name: external-lb
spec:
  controller: example.com/ingress-controller
  parameters:
    apiGroup: k8s.example.com
    kind: IngressParameters
    name: external-lb
```

<small>Fonte: [Kubernetes Documentation](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls)</small>