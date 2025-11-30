---
title: "Trackunit: kind Cluster Configuration"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: da
locale: da
nav_order: 38
ref: kind-cluster-config
---

Dette manifest definerer konfigurationen for det lokale Kubernetes-cluster i **kind**. Det angiver en separat `control-plane`- og `worker`-node samt deaktiverer kind’s default CNI (`disableDefaultCNI: true`), så **Calico** kan installeres som netværksplugin. Derudover sættes et `podSubnet`, der matcher Calicos standard IP-range (`192.168.0.0/16`), så netværkslaget fungerer korrekt efter installationen.

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