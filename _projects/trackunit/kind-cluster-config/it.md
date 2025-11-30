---
title: "Trackunit: Configurazione del Cluster kind"
categories: [trackunit, kubernetes]
tags: [yaml, configuration, final]
lang: it
locale: it
nav_order: 38
ref: kind-cluster-config
---

Questo manifest definisce la configurazione del cluster Kubernetes locale in **kind**. Specifica un nodo `control-plane` e un nodo `worker` separati e disabilita il CNI predefinito di kind (`disableDefaultCNI: true`) affinché **Calico** possa essere installato come plugin di rete. Inoltre imposta un `podSubnet` che corrisponde all’intervallo IP predefinito di Calico (`192.168.0.0/16`), garantendo il corretto funzionamento del livello di rete dopo l’installazione.

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