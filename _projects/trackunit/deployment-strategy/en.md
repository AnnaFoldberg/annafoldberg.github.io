---
title: "Trackunit: Deployment Strategy"
categories: [trackunit, kubernetes]
tags: [rollingupdate, replicas, deployment, final]
lang: en
locale: en
nav_order: 47
ref: deployment-strategy
---

###### graph-gateway

Runs with `3` replicas and uses the following strategy:

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 1
```

This strategy is chosen because the gateway is the system’s entry point.  
With three pods, one pod can be taken down at a time without losing capacity or making the service unavailable.

###### svc-analysis-orchestrator

Runs with `2` replicas and uses a more conservative strategy:

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 0
```

This strategy is selected because the orchestrator handles internal analysis logic but is not directly on the client’s request path.  
It is important that the service always maintains two available pods, enabling it to continue processing even if a pod restarts.

##### Helm-deployed components

**Kong** and **oauth2-proxy** use the default upgrade strategies defined in their Helm charts:

- **Kong:** RollingUpdate (`25%` maxUnavailable, `25%` maxSurge)  
- **oauth2-proxy:** RollingUpdate (`25%` maxUnavailable, `25%` maxSurge)

`maxUnavailable` is calculated as `floor(replicas * maxUnavailable)`, and  
`maxSurge` as `ceil(replicas * maxSurge)`.

Since each service only has one replica, `25%` effectively means that Kubernetes always creates the new pod **before** removing the old one → resulting in zero downtime.

**RabbitMQ** also uses the upgrade strategy defined in its Helm chart.  
Because it is a StatefulSet, the strategy is:

- **RabbitMQ (StatefulSet):** RollingUpdate

StatefulSets ignore `maxUnavailable` and `maxSurge` and never update multiple pods simultaneously.  
Kubernetes creates a new pod, waits for readiness, and then removes the old one → ensuring zero downtime as well.