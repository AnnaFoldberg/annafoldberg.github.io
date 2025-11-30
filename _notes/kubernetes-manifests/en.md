---
title: "Kubernetes Manifests"
categories: [kubernetes]
tags: [manifests, pods, replicasets, deployments]
lang: en
locale: en
nav_order: 57
ref: note-kubernetes-manifests
---

### Kubernetes Manifests

Kubernetes manifests are used to describe the desired state of workloads in a cluster. They are declarative YAML files that Kubernetes ensures will always match the actual state. Three of the most fundamental resources are Pods, ReplicaSets, and Deployments.

![Manifests](../../assets/images/notes/kubernetes-manifests/manifests.png)

**Pod**

A Pod is the smallest deployable unit in Kubernetes and contains one or more containers. In microservices architectures, a 1:1 relationship is almost always used, where a pod contains only one container, because microservices are kept separate and are easy to scale individually.

The illustration shows an example Pod manifest describing an nginx container. Under metadata, the name, labels, and optionally the namespace are defined. Nginx here acts as a regular web server running in the container.

**ReplicaSet**

A ReplicaSet ensures that a specific number of pods, defined in the replicas field, are running at all times. If a pod fails, the ReplicaSet automatically creates a new one to maintain the desired count.

The middle manifest shows a ReplicaSet with three replicas, corresponding to three identical pods.

**Deployment**

A Deployment manages one or more ReplicaSets and defines the strategy for how pods should be updated. The left manifest shows a Deployment that again describes three replicas of an nginx pod.

The difference between Deployment and ReplicaSet is that Deployment includes upgrade strategies, while ReplicaSet only ensures that x number of pods remain running.

In practice, you almost always create only the Deployment manifest. Kubernetes automatically creates both the ReplicaSet and the pods. It is considered bad practice to create pods directly, because they are not recreated on failure. A ReplicaSet could recreate pods, but without the benefits of an update strategy.

**Update Strategy**

In the Deployment example, the strategy RollingUpdate is used. It controls how new versions of pods are rolled out. The settings:

- `maxUnavailable: 1` means that at most one pod may be unavailable during an update.
- `maxSurge: 1` means that one additional pod may be created temporarily.

During an update there may therefore briefly be four pods: one old pod being terminated, one new pod being started, and two existing pods that are fully operational. This provides a controlled and stable update without downtime.