---
title: "Network Policies"
categories: [kubernetes]
tags: [network-policies, networking, security, pods]
lang: en
locale: en
nav_order: 61
ref: note-network-policies
---

##### PolicyTypes

There are two types of NetworkPolicy rules:

- **Ingress:** Controls which incoming traffic is allowed to reach a pod.  
- **Egress:** Controls which outgoing traffic a pod is allowed to send.

##### Applying Network Policies

A NetworkPolicy only applies to pods in the namespace where the policy is defined.

Namespaces are selected with `namespaceSelector`, and pods are selected with `podSelector`. With `ipBlock`, traffic can be restricted based on IP ranges. Incoming or outgoing connections are controlled through the sources or destinations specified under `ingress:` or `egress:`.

![Network Policy](../../assets/images/notes/network-policies/network-policy.png)

In the example, namespace-b only accepts traffic to pods with the label `environment: test` from pods in namespace-a, because the `namespaceSelector` matches the label `myspace: namespacea`.

Network Policies require a network policy agent in the cluster to be enforced. These are **not** installed automatically in Kubernetes. An example of a CNI (Container Network Interface) that supports Network Policies is **Calico**. A CNI defines how containers connect to the network, and how plugins such as Calico manage networking inside Kubernetes.

##### OR and AND Conditions

![Network Policy: OR & AND](../../assets/images/notes/network-policies/and-or-conditions.png)

Rules under `from:` or `to:` are interpreted based on how list items are structured:

- **OR-condition:** Each criterion set is written as a separate list item (`-`). A source only needs to match *one* of the listed criteria.  
- **AND-condition:** `ipBlock`, `namespaceSelector`, and `podSelector` appear in the *same* list item. A source must match *all* criteria in that single item.

<small>Source: [Kubernetes Network Policy Tutorial](https://www.youtube.com/watch?v=u1KUft3fsCk)</small>