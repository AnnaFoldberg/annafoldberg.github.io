---
title: "Nodes i kind"
categories: [kubernetes]
tags: [kind, clusters, worker-nodes, control-plane]
lang: da
locale: da
nav_order: 56
ref: note-kind-nodes
---

Når man opretter et cluster med `kind create cluster` uden en config-fil, opretter Kind:

- 1 node med rollen control-plane  
- 0 separate worker-noder  

Control-plane noden fungerer både som styrende node og som worker-node, da control-plane i Kubernetes godt kan køre pods, og Kind som standard tillader dette. Derfor består clusteret af én node, som udfører begge roller.

Selvom en control-plane node godt kan køre pods, vil et produktionscluster typisk have en anden konfiguration:

- Control-plane noder er tainted med `node-role.kubernetes.io/control-plane:NoSchedule` som standard, så workloads ikke schedules der  
- Separate worker-noder kører alle pods  

<small>Kilde: [kind Documentation](https://kind.sigs.k8s.io/docs/user/configuration/#nodes)</small>