---
title: "Checking Pod Health with Event Logs"
categories: [kubernetes]
tags: [pods, health, logs]
lang: en
locale: en
nav_order: 42
ref: note-pod-health-event-logs
---
The beginning of a Pod’s lifecycle is the most fragile stage. Failures can occur if the container image is unavailable, if there is insufficient space on worker nodes, or if the Pod starts but then crashes. Kubernetes saves event logs when a Pod is created, which can be used for troubleshooting.

##### Steps
**1. List Pods in a namespace:** Run `kubectl get pods -n development`. Copy the name of the Pod to investigate.  

**2. Describe the Pod:** Run `kubectl describe pod <pod-name> -n development`. Scroll to the bottom of the output to view the event logs.  

##### Interpreting Events
- If the Pod is healthy, events will confirm normal scheduling and container startup.  
- If the Pod encounters issues, error messages will appear in the events to help identify the problem.  
- If the Pod has been running for some time without issues, no recent events will be shown.  

##### Key Point
Most Pod issues occur in the first minute of their lifecycle. After that, a lack of events usually indicates the Pod is healthy.

<small> Source: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>