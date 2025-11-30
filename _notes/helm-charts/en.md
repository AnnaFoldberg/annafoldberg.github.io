---
title: "Helm Charts in Kubernetes"
categories: [kubernetes]
tags: [helm, charts, packages, manifests]
lang: en
locale: en
nav_order: 62
ref: note-helm-charts
---

Helm is a package manager for Kubernetes that makes it easier to install and manage applications in a cluster. While Kubernetes normally requires multiple manifests (deployments, services, etc.), Helm bundles these into a single package called a Helm Chart.

A Helm Chart defines all Kubernetes resources that belong to an application and allows installing, upgrading, and uninstalling them as a single unit.

A Helm Chart typically consists of:

- Templated YAML manifests  
- A values file for configuration  
- A Chart.yaml containing metadata  

When installing a chart, Helm fills templates with values, generates complete Kubernetes manifests, and applies them to the cluster.

##### Community Charts

Helm supports reusing community-maintained charts for popular technologies. When installing something like Kong or RabbitMQ via Helm (e.g., from https://artifacthub.io), you fetch an existing chart and automatically get all required resources created in the cluster.

##### Custom Charts

You can also create your own Helm Charts for internal services. This is useful when an application consists of many resources, has complex configuration, or needs to be deployed consistently across multiple environments.

For simple applications with only a few manifests, custom charts often add little value and may introduce unnecessary complexity.

<small>Source: [Helm Documentation](https://helm.sh/)</small>