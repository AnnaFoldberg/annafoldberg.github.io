---
title: "Running Stateful Workloads"
categories: [kubernetes]
tags: [databases, persistent-volumes]
lang: en
locale: en
nav_order: 53
ref: note-running-stateful-workloads
---
Handling data storage is an important consideration when running applications in Kubernetes. Stateful workloads can be managed in two main ways: by integrating with an external database or by using Persistent Volumes within the cluster, both of which ensure data persists beyond the lifecycle of individual Pods.

##### External Database
Applications can connect to a database running outside the cluster. For example, an application that uses PostgreSQL can either rely on a standalone SQL database hosted on a separate server or use a managed service such as Azure SQL, Amazon RDS, or Google Cloud SQL. The database is configured to communicate with Kubernetes while remaining independent of the cluster.

##### Persistent Volumes
Kubernetes offers Persistent Volumes to provide storage that remains even after a Pod is destroyed. A StatefulSet can be used to ensure Pods consistently connect to the same Persistent Volume, preserving data across Pod restarts or updates.

<small> Source: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>