---
title: "Namespaces"
categories: [kubernetes]
tags: [namespaces, containers, isolation]
lang: en
locale: en
nav_order: 59
ref: note-namespaces-container-vs-kubernetes
---

##### Container Namespaces

Containers are isolated from the host using namespaces. A namespace restricts which processes the container can see, preventing it from accessing processes on the host or in other containers. To the container, it appears as if it is the only system running. The host, however, has full access to all processes, including those running inside containers.

##### Namespaces in Kubernetes

Kubernetes namespaces are used to logically divide resources within a cluster. A namespace groups related objects, separates environments such as dev, test, and prod, and prevents naming conflicts.

Workloads inside a namespace can only access resources within that namespace. However, pods can still communicate across namespaces unless restricted with NetworkPolicies. Namespaces therefore support both separation and controlled communication.

Because namespaces in Kubernetes function as logical divisions of the cluster, they do not change the container’s actual isolation on the host. They help organize resources but do not prevent pods from seeing or contacting each other on the underlying network. Therefore, namespaces are not a security boundary at the node level.

In Kubernetes, resources such as pods, services, and deployments always belong to a specific namespace. If no namespace is specified, they are automatically placed in the default namespace.

<small>Source: [Udemy Course: Certified Kubernetes Administrator](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/14296142#overview)</small>