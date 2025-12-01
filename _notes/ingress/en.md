---
title: "Ingress"
categories: [kubernetes]
tags: [ingress, tls, networking, routing]
lang: en
locale: en
nav_order: 65
ref: note-kubernetes-ingress
---

##### Ingress

An Ingress is a Kubernetes resource that exposes HTTP and HTTPS traffic from outside the cluster to internal Services. Instead of opening ports directly on a Service, an Ingress defines rules for routing based on paths or hostnames.

An Ingress object requires an Ingress Controller, which translates the Ingress rules into actual load balancer configurations.

##### TLS

An Ingress can be secured with TLS by referencing a Secret containing a certificate and private key. Ingress only handles TLS on port 443, and traffic to Services afterward is expected to be unencrypted-TLS is terminated at the Ingress layer.

The TLS section specifies which hosts should use the certificate. If multiple hosts are configured, routing occurs via the SNI extension, provided the controller supports it.

The certificate inside the referenced Secret must be named `tls.crt` and `tls.key`, and the domain (CN/FQDN) must match the host in the Ingress rule.

>**SNI:** The mechanism that allows multiple HTTPS domains to be hosted on the same IP by letting the client reveal the hostname early in the TLS handshake.

**Example of a simple Ingress that routes traffic to a single Service with TLS**  

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

Multiple Ingress Controllers can run in the same cluster, so each Ingress should specify which class it belongs to. An IngressClass defines which controller should handle Ingress objects that use this class, including any additional configuration.

An IngressClass links an Ingress object to the specific controller implementation.

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

<small>Source: [Kubernetes Documentation](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls)</small>