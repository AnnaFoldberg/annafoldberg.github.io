---
title: "Trackunit: Kubernetes Cluster Architecture"
categories: [trackunit, kubernetes]
tags: [architecture, cluster, diagram, final]
lang: da
locale: da
nav_order: 52
ref: kubernetes-cluster-architecture
---

![kubernetes-cluster-architecture.png](../../../assets/images/projects/trackunit/kubernetes-cluster-architecture/kubernetes-cluster-architecture.png)

> **Tip:** Åbn diagrammet i en ny fane for at zoome ind og se detaljer.

Dette diagram giver et overblik over Kubernetes-clusterets struktur i **kind**. Det viser separationen mellem control plane (**kind-control-plane**) og data plane (**kind-worker**).

##### kind-control-plane

![control-plane.png](../../../assets/images/projects/trackunit/kubernetes-cluster-architecture/control-plane.png)

Kører control plane komponenter, herunder **etcd**, **kube-controller-manager**, **kube-apiserver**, **kube-scheduler**, samt cluster services som **coredns**, **calico-kube-controllers** og **local-path-provisioner**.

Den anses også som en node og kører derfor systempods **calico-node** og **kube-proxy**.

**cloud-provider-kind** kører som en ekstern **cloud-controller-manager** uden for clusteret og tildeler eksterne IP’er til **kong-kong-proxy** LoadBalancer-servicen.

I **etcd** lagres **Kubernetes API Objects**, herunder **deployments**, **services**, **namespaces**, **ingress**, **secrets**, **network policies**, **daemonSets**, **replicaSets**, **statefulSets** og **persistentVolumeClaims**.

##### kind-worker

![data-plane.png](../../../assets/images/projects/trackunit/kubernetes-cluster-architecture/data-plane.png)

Kører applikationspods — **graph-gateway**, **svc-analysis-orchestrator**, **oauth2-proxy**, **kong-kong** og **rabbitmq** — samt systempods **calico-node** og **kube-proxy** i container-runtime. Kører nodeprocessen **kubelet**.

##### Interaktioner mellem komponenter og nodes

De fleste control plane- og nodekomponenter kommunikerer med Kubernetes via **kube-apiserver**.  
**kubelet** kommunikerer begge veje med **kube-apiserver**, mens **etcd** kun tilgås direkte af **kube-apiserver**.