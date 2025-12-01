---
title: "Nodes in kind"
categories: [kubernetes]
tags: [kind, clusters, worker-nodes, control-plane]
lang: en
locale: en
nav_order: 56
ref: note-kind-nodes
---

When creating a cluster with `kind create cluster` without a config file, Kind creates:

- 1 node with the control-plane role  
- 0 separate worker nodes  

The control-plane node acts as both the control-plane and a worker node, because control-plane nodes in Kubernetes are capable of running pods, and Kind allows this by default. As a result, the cluster consists of a single node performing both roles.

In a production cluster, the configuration is typically different:

- Control-plane nodes are tainted with `node-role.kubernetes.io/control-plane:NoSchedule` to prevent workloads from being scheduled there  
- Separate worker nodes run all application pods  

<small>Source: [kind Documentation](https://kind.sigs.k8s.io/docs/user/configuration/#nodes)</small>