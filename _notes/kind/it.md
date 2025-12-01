---
title: "Kind"
categories: [kubernetes]
tags: [kind, clusters]
lang: it
locale: it
nav_order: 55
ref: note-kind
---

Kind è uno strumento che permette di eseguire cluster Kubernetes localmente utilizzando container Docker come nodi. È stato originariamente progettato per testare Kubernetes, ma può essere utilizzato per sviluppo locale, test e proof-of-concept in cui è necessario un vero ambiente Kubernetes senza dipendere da un cloud provider.

Kind crea un cluster completo all’interno di Docker, dove ogni nodo del cluster corrisponde a un container separato.

##### IP esterni in Kind

Poiché Kind non viene eseguito in un ambiente cloud, i load balancer esterni non vengono creati automaticamente. Per utilizzare il tipo di Service LoadBalancer in Kind, è necessario aggiungere un componente che simuli un cloud provider. Questo permette di testare componenti come gli API gateway, che normalmente richiedono un load balancer esterno.

In alternativa, è possibile utilizzare il tipo di Service NodePort per lo sviluppo locale.

##### Comandi Kind

- Creare un cluster: `kind create cluster`  
- Eliminare un cluster: `kind delete cluster`  
- Simulare un cloud provider: `sudo go/bin/cloud-provider-kind`

<small>Fonte: [kind Documentation](https://kind.sigs.k8s.io/)</small>