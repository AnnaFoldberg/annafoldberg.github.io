---
title: "2025-11-05: Progress and Direction"
categories: [microservices, kubernetes, it-security]
tags: [individual, process, feedback, feed-forward]
lang: en
locale: en
nav_order: 15
ref: log-2025-11-05-progress-direction
---

**Feedback**

In my work on the Trackunit project, I have built a microservices environment based entirely on Docker and docker-compose. The goal at this stage was to get all services working together in a local setup and understand how they communicate across containers.

The core services at this point are **GraphGateway** and **SvcAnalysisOrchestrator**, both of which are built using Dockerfiles and published as images to GitHub Container Registry through a CI workflow.

**GraphGateway** serves as the API façade for the system. It exposes a GraphQL API (queries, mutations and subscriptions) and sends `RequestAnalysis` commands to RabbitMQ. It also receives the `analysis.started` and `analysis.completed` events and forwards them to the subscription fields `onAnalysisStarted` and `onAnalysisCompleted`.

**SvcAnalysisOrchestrator** orchestrates analyses. It consumes `RequestAnalysis` commands from RabbitMQ and publishes `analysis.started` and `analysis.completed` events. At this stage, it simulates the analysis itself with a short delay.

**InfraCore** bundles all services and infrastructure components into a single docker-compose environment, allowing the entire system to start together and communicate through a shared Docker network.

**ClientConsole** serves as a temporary client for testing the authentication–GraphQL flow and verifying that messages are passed correctly through the system.

**Feed-forward**

My next step will be to move operations from docker-compose to Kubernetes manifests. This will allow for scaling, better isolation of services, NetworkPolicies, rolling updates, and a more robust and flexible operational model.