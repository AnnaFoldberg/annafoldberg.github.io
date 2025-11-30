---
title: "Trackunit: kind Cluster Configuration"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: en
locale: en
nav_order: 38
ref: kind-cluster-config
---

This manifest defines the configuration for the local Kubernetes cluster in **kind**. It specifies a separate `control-plane` and `worker` node and disables kind’s default CNI (`disableDefaultCNI: true`) so that **Calico** can be installed as the network plugin. Additionally, it sets a `podSubnet` that matches Calico’s default IP range (`192.168.0.0/16`), ensuring the network layer functions correctly after installation.

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
networking:
  disableDefaultCNI: true
  podSubnet: "192.168.0.0/16" # matches Calico's default pool
nodes:
- role: control-plane
- role: worker
```