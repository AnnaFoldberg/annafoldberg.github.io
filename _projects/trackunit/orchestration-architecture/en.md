---
title: "Trackunit: Analysis Orchestrator's Role"
categories: [trackunit, microservices]
tags: [architecture, messaging, events, commands, final]
lang: en
locale: en
nav_order: 49
ref: orchestration-architecture
---

**svc-analysis-orchestrator** is designed to be the central coordination component for the entire analysis flow. Although the current implementation is simplified for early development, the architecture is prepared for a scaled setup where the orchestrator drives the process through explicit commands and domain-specific events.

The goal is to have one service that knows exactly what must happen in the analysis, when it must happen, and when the entire workflow is complete.

This allows every analysis service to remain small, specialized, and independent, while the orchestrator manages the overarching flow.

##### Full orchestration in a scaled setup

###### Commands

The orchestrator issues commands to trigger each analysis step:

- `analysis.recognition.request`: Initiates image recognition in **svc-ai-vision-adapter**
- `analysis.comparison.request`: Starts comparison of recognition results
- `analysis.metadata.request`: Initiates metadata analysis or sorting

###### Events

Each analysis service emits events when their part of the workflow is complete:

- `analysis.started`: The orchestrator has received `tu.image.uploaded` and begun the analysis.
- `recognition.completed`: Emitted by **svc-ai-vision-adapter**. Consumed by:
  - **graph-gateway**, which emits a corresponding GraphQL event to **trackunit-client**.
  - **svc-analysis-orchestrator**, which logs progress and triggers the next step.
- `comparison.completed`: Emitted by **comparison-service**. Consumed by:
  - **graph-gateway**
  - **svc-analysis-orchestrator**
- `metadata.sorted`: Emitted by **metadata-service**. Consumed by:
  - **graph-gateway**
  - **svc-analysis-orchestrator**, which then emits `analysis.completed`.
- `analysis.completed`: Emitted by **svc-analysis-orchestrator** when the workflow is finished. Consumed by:
  - **graph-gateway**
  - **svc-machine-storage**, which persists machine data.

Every success event has a corresponding `*.failed` event. These are used by **svc-analysis-orchestrator** and **graph-gateway** to handle failure scenarios and update **trackunit-client**.

##### Benefits of this architecture

- Commands are sent point-to-point to a single responsible service.  
- Events are broadcast so multiple components can react as needed.  
- **svc-analysis-orchestrator** controls the sequence but performs no analysis work.  
- Analysis services remain small, specialized, and easy to scale.  
- Persistence is handled outside the orchestrator, keeping the workflow unblocked.  
- **trackunit-client** receives only events, never internal commands.  
- The architecture allows services and analysis stages to scale independently.