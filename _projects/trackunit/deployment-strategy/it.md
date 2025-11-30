---
title: "Trackunit: Strategia di Deployment"
categories: [trackunit, kubernetes]
tags: [rollingupdate, replicas, deployment, final]
lang: it
locale: it
nav_order: 47
ref: deployment-strategy
---

###### graph-gateway

Viene eseguito con `3` repliche e utilizza la seguente strategia:

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 1
```

Questa strategia è scelta perché la gateway rappresenta il punto di ingresso del sistema.  
Con tre pod, è possibile sostituirne uno alla volta senza perdere capacità o rendere il servizio indisponibile.

###### svc-analysis-orchestrator

Viene eseguito con `2` repliche e utilizza una strategia più conservativa:

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 0
```

Questa strategia è scelta perché l’orchestrator gestisce la logica interna di analisi, ma non si trova direttamente nel request-path del client.  
È importante mantenere sempre due pod disponibili, così il servizio può continuare l’elaborazione anche se uno di essi si riavvia.

##### Componenti distribuiti tramite Helm

**Kong** e **oauth2-proxy** utilizzano le strategie di aggiornamento predefinite dai rispettivi Helm chart:

- **Kong:** RollingUpdate (`25%` maxUnavailable, `25%` maxSurge)  
- **oauth2-proxy:** RollingUpdate (`25%` maxUnavailable, `25%` maxSurge)

`maxUnavailable` viene calcolato come `floor(replicas * maxUnavailable)` e  
`maxSurge` come `ceil(replicas * maxSurge)`.

Poiché ogni servizio ha una sola replica, il `25%` significa che Kubernetes crea sempre un nuovo pod **prima** di rimuovere quello vecchio → nessun downtime.

Anche **RabbitMQ** utilizza la strategia definita dal proprio Helm chart.  
Essendo un StatefulSet, la strategia è:

- **RabbitMQ (StatefulSet):** RollingUpdate

Gli StatefulSet ignorano `maxUnavailable` e `maxSurge` e non aggiornano mai più pod contemporaneamente.  
Kubernetes crea un nuovo pod, attende che sia pronto e rimuove quello vecchio → garantendo anche qui zero downtime.