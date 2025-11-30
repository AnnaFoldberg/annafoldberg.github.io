---
title: "Kubernetes Update Strategies"
categories: [kubernetes]
tags: [deployments, rolling-update, recreate, replicasets]
lang: en
locale: en
nav_order: 62
ref: note-upgrade-strategies
---

Kubernetes offers two strategies for updating an application in a Deployment:

##### Recreate  
- All existing pods are deleted at once.  
- Once the old pods are removed, new pods with the updated version are created.  
- The application experiences downtime during the transition.

##### RollingUpdate (default)  
RollingUpdate replaces pods gradually to avoid downtime. It is controlled by:

- **maxSurge:** How many additional pods may be created temporarily.  
- **maxUnavailable:** How many pods may be taken down at the same time.  

Both values can be numbers (e.g., 1) or percentages (e.g., 25%).  
This enables controlled upgrades where the application stays available while pods are replaced one-by-one.

If no strategy is specified, Kubernetes defaults to RollingUpdate with `maxSurge: 1` and `maxUnavailable: 1`.

##### RollingUpdate in Practice

![Deployment Structure](../../assets/images/notes/upgrade-strategies/deployment-structure.png)

The image shows a Deployment in normal operation.

![New ReplicaSet](../../assets/images/notes/upgrade-strategies/new-replicaset.png)

When the Deployment version changes, Kubernetes automatically creates a new ReplicaSet. It then begins phasing out pods from the old one and creating new pods in the new ReplicaSet.

![Rolling Transition](../../assets/images/notes/upgrade-strategies/rolling-transition.png)

With both `maxSurge` and `maxUnavailable` set to 1, Kubernetes scales one old pod down and one new pod up at a time. The remaining old pods keep the application running without interruption.

![Mixed Traffic Phase](../../assets/images/notes/upgrade-strategies/mixed-traffic.png)

During transition, traffic is split between the old and new versions. The application continues functioning even though users may hit different versions.

![Finalization](../../assets/images/notes/upgrade-strategies/finalization.png)

Once all new-version pods are running, the old ReplicaSet can be removed. Kubernetes usually keeps it temporarily to support quick rollbacks.