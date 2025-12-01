---
title: "Ingress"
categories: [kubernetes]
tags: [ingress, tls, networking, routing]
lang: da
locale: da
nav_order: 65
ref: note-kubernetes-ingress
---

##### Ingress

En Ingress er en Kubernetes-ressource, der eksponerer HTTP- og HTTPS-trafik fra omverdenen til Services inde i clusteret. I stedet for at åbne porte direkte på en Service, beskriver en Ingress regler for, hvordan trafik skal routes baseret på stier eller domænenavne.

Ingress-objektet kræver en Ingress Controller, der omsætter Ingress-reglerne til faktiske load balancer-konfigurationer.

##### TLS

En Ingress kan sikres med TLS ved at referere til et Secret, der indeholder et certifikat og en privat nøgle. Ingress håndterer kun TLS på port 443 og forventer, at trafik til Services efterfølgende foregår uden kryptering, det vil sige TLS termineres ved Ingress-laget.

TLS-sektionen i en Ingress angiver hvilke hosts, der skal bruge certifikatet. Hvis flere hosts er angivet, sker routing via SNI-udvidelsen, forudsat at controlleren understøtter det.

Certifikatet i det refererede Secret skal hedde `tls.crt` og `tls.key`, og domænenavnet (CN/FQDN) skal matche hosten i Ingress-reglen.

>**SNI:** Mekanismen, der gør det muligt at hoste flere HTTPS-domæner på samme IP ved at lade klienten afsløre domænenavnet tidligt i TLS-handshaket.

**Eksempel på en simpel Ingress, der sender trafik til en enkelt Service med TLS**  

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

Forskellige Ingress Controllers kan eksistere i samme cluster, og hver Ingress bør derfor specificere hvilken klasse den hører til. En IngressClass beskriver hvilken controller, der skal håndtere de Ingress-objekter, der bruger denne klasse, samt eventuelle ekstra konfigurationer.

En IngressClass binder dermed et Ingress-objekt sammen med den konkrete controller-implementering.

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

<small>Kilde: [Kubernetes Documentation](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls)</small>