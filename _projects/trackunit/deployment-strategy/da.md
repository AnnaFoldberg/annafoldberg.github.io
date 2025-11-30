---
title: "Trackunit: Deployment Strategi"
categories: [trackunit, kubernetes]
tags: [rollingupdate, replicas, deployment, final]
lang: da
locale: da
nav_order: 47
ref: deployment-strategy
---

###### graph-gateway

Kører med `3` replicas og benytter følgende strategi:

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 1
```

Denne strategi er valgt, fordi gatewayen er indgangspunktet til systemet.  
Med tre pods kan én pod tages ud ad gangen, uden at servicen mister kapacitet eller bliver utilgængelig.

###### svc-analysis-orchestrator

Kører med `2` replicas og benytter en mere konservativ strategi:

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 0
```

Denne strategi er valgt, fordi orchestratoren håndterer intern analyselogik, men ikke ligger direkte i klientens request-path.  
Det er vigtigt, at servicen altid har mindst to pods tilgængelige, så den kan fortsætte arbejdet, selv hvis en pod genstarter.

##### Helm-deployede komponenter

**Kong** og **oauth2-proxy** anvender de opdateringsstrategier, som deres Helm charts angiver som standard:

- **Kong:** RollingUpdate (`25%` maxUnavailable, `25%` maxSurge)  
- **oauth2-proxy:** RollingUpdate (`25%` maxUnavailable, `25%` maxSurge)

`maxUnavailable` beregnes som `floor(replicas * maxUnavailable)`, og  
`maxSurge` beregnes som `ceil(replicas * maxSurge)`.

Eftersom hver service kun har én replica, betyder `25%` på begge værdier, at Kubernetes altid starter en ny pod **før** den fjerner den gamle → ingen nedetid.

Også **RabbitMQ** anvender den opgraderingsstrategi, som dets Helm chart angiver som standard.  
Da det er et StatefulSet, ser strategien således ud:

- **RabbitMQ (StatefulSet):** RollingUpdate

StatefulSets ignorerer `maxUnavailable` og `maxSurge` og opdaterer aldrig flere pods samtidig.  
Kubernetes opretter en ny pod, venter på at den er klar, og fjerner først derefter den gamle → også dette giver opdatering uden nedetid.