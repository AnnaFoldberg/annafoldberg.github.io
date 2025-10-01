---
title: "Ways to Manage Kubernetes Pods"
categories: [kubernetes]
tags: [pods, controllers]
lang: en
locale: en
nav_order: 52
ref: note-manage-kubernetes-pods
---
Kubernetes provides several controllers for managing Pods, each designed for specific use cases. Deployments, DaemonSets, and Jobs represent three key strategies: ensuring continuous availability, running agents on every node, or executing workloads that run to completion.

##### Deployment
The most common way to run containerized applications. Deployments control the number of replicas and support rolling updates. When a new version of an application is deployed, Kubernetes keeps the old version running, rolls out the new one, verifies Pod health, and then removes the old Pods. This enables no-downtime upgrades.

##### DaemonSet
Ensures that one copy of a Pod runs on every node in the cluster. Replica counts cannot be directly controlled. DaemonSets are typically used for background processes, such as agents that collect metrics about nodes and Pods.

##### Job
Runs one or more Pods until a specified task completes successfully. Jobs are useful for workloads that need to run to completion, such as generating test data in a batch process. Once complete, the Pods can be deleted.

<small> Source: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>