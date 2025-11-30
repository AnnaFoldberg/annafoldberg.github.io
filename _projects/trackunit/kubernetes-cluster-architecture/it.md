---
title: "Trackunit: Kubernetes Cluster Architecture"
categories: [trackunit, kubernetes]
tags: [architecture, cluster, diagram, final]
lang: it
locale: it
nav_order: 52
ref: kubernetes-cluster-architecture
---

![kubernetes-cluster-architecture.png](../../../assets/images/projects/trackunit/kubernetes-cluster-architecture/kubernetes-cluster-architecture.png)

> **Suggerimento:** Apri il diagramma in una nuova scheda per ingrandire e vedere i dettagli.

Questo diagramma fornisce una panoramica dell’architettura del cluster Kubernetes in **kind**. Mostra la separazione tra il control plane (**kind-control-plane**) e il data plane (**kind-worker**).

##### kind-control-plane

![control-plane.png](../../../assets/images/projects/trackunit/kubernetes-cluster-architecture/control-plane.png)

Esegue i componenti del control plane, inclusi **etcd**, **kube-controller-manager**, **kube-apiserver**, **kube-scheduler**, oltre ai servizi di cluster come **coredns**, **calico-kube-controllers** e **local-path-provisioner**.

È considerato anche un nodo e quindi esegue i pod di sistema **calico-node** e **kube-proxy**.

**cloud-provider-kind** viene eseguito esternamente come **cloud-controller-manager** e assegna IP esterni al servizio LoadBalancer **kong-kong-proxy**.

In **etcd** vengono memorizzati gli **oggetti Kubernetes API**, tra cui **deployments**, **services**, **namespaces**, **ingress**, **secrets**, **network policies**, **daemonSets**, **replicaSets**, **statefulSets** e **persistentVolumeClaims**.

##### kind-worker

![data-plane.png](../../../assets/images/projects/trackunit/kubernetes-cluster-architecture/data-plane.png)

Esegue i pod applicativi — **graph-gateway**, **svc-analysis-orchestrator**, **oauth2-proxy**, **kong-kong** e **rabbitmq** — oltre ai pod di sistema **calico-node** e **kube-proxy** nel container runtime. Esegue anche il processo del nodo **kubelet**.

##### Interazioni tra componenti e nodi

La maggior parte dei componenti del control plane e dei nodi interagisce con Kubernetes tramite **kube-apiserver**.  
**kubelet** comunica in entrambe le direzioni con **kube-apiserver**, mentre **etcd** viene accesso direttamente da **kube-apiserver**.