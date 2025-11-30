---
title: "2025-11-14: Progress and Direction"
categories: [microservices, kubernetes, it-security]
tags: [individual, process, feedback, feed-forward]
lang: en
locale: en
nav_order: 17
ref: log-2025-11-14-progress-direction
---

##### Feedback

I have now moved the Trackunit environment from docker-compose into Kubernetes.

I have created deployments, services, secrets, namespaces, network policies, and Helm values files for use with Helm charts. All microservices run as pods managed by deployments and are exposed internally via ClusterIP services, while only Kong is exposed externally through a LoadBalancer service. Infrastructure components such as **kong**, **rabbitmq**, and **oauth2-proxy** run as Kubernetes resources via Helm. This has made the overall setup more flexible, scalable, and robust.

I use `kind` to run my local Kubernetes cluster, as it provides a realistic environment and makes it possible to test Kong’s LoadBalancer service through `cloud-provider-kind`. Kong is the only service exposed outside the cluster and serves as the entry point to the system.

To manage and monitor the cluster, I use `k9s`, which provides a fast and efficient overview of deployments, pods, logs, and events.

##### Feed-forward

My next focus will be to introduce versioning for container images, so I can better manage releases and allow Kubernetes to perform clean rolling updates when upgrading to new image versions.

After that, I will implement TLS termination between the client and **kong-proxy**, ensuring that all traffic outside the cluster is encrypted in transit.

I choose not to introduce mTLS, neither by requiring a client certificate nor internally between pods in Kubernetes via a service mesh, due to the size of the project and because internal traffic is already protected externally by the API gateway. Even in a production environment, an attacker would have to get past both the API gateway and the authorisation in **graph-gateway** before any internal pod-to-pod communication could be accessed.  

Once the remaining microservices are implemented, **svc-analysis-orchestrator** will be extended to manage the analysis flow after upload. A messaging bridge will also be introduced, functioning as an adapter between RabbitMQ and Kafka, as my existing microservices use RabbitMQ while the new services rely on Kafka. At the same time, **graph-gateway** will be expanded with GraphQL endpoints that forward requests to tu-ingestion-service via its REST endpoints and handle the responses returned.