---
title: "Network Policies"
categories: [kubernetes]
tags: [network-policies, networking, security, pods]
lang: da
locale: da
nav_order: 61
ref: note-network-policies
---

##### PolicyTypes

Der findes to typer NetworkPolicy-regler:

- **Ingress:** Styrer hvilken indgående trafik der må nå en pod.  
- **Egress:** Styrer hvilken udgående trafik en pod må sende.

##### Anvendelse af Network Policies

En NetworkPolicy gælder kun for pods i det namespace, hvor policyen er defineret.

Namespaces vælges med namespaceSelector, og pods vælges med podSelector. Med ipBlock kan man begrænse trafik baseret på IP-adresser. Indgående eller udgående forbindelser styres gennem de kilder eller destinationer, der angives under `ingress:` eller `egress:`.

![Network Policy](../../assets/images/notes/network-policies/network-policy.png)

I eksemplet accepterer namespace-b kun trafik til pods med label `environment: test` fra pods i namespace-a, fordi namespaceSelector matcher labelen `myspace: namespacea`.

Network Policies kræver en network policy agent i clusteret for at blive håndhævet. Disse installeres ikke automatisk i Kubernetes. Et eksempel på en CNI (Container Network Interface), der understøtter Network Policies, er Calico. Et CNI definerer, hvordan containere tilkobles netværket, og hvordan plugins såsom Calico håndterer netværk i Kubernetes.

##### OR- og AND-betingelser

![Network Policy: OR & AND](../../assets/images/notes/network-policies/and-or-conditions.png)

Regler under `from:` eller `to:` fortolkes afhængigt af brugen af listepunkter:

- **OR-condition:** Hvert kriteriesæt angives som et separat listepunkt (`-`). Kilden skal matche ét af kriterierne.  
- **AND-condition:** `ipBlock`, `namespaceSelector` og `podSelector` står i det samme listepunkt. Kilden skal matche alle kriterier i dette listepunkt.

<small>Kilde: [Kubernetes Network Policy Tutorial](https://www.youtube.com/watch?v=u1KUft3fsCk)</small>