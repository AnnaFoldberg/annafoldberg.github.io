---
title: "Verifying Applications with BusyBox"
categories: [kubernetes]
tags: [busybox, debugging, troubleshooting]
lang: en
locale: en
nav_order: 43
ref: note-verifying-applications-busybox
---
After deploying an application in Kubernetes, it is often necessary to verify that it is working correctly. One way to do this is by using BusyBox, a lightweight Linux utility that bundles many common Unix tools (e.g., awk, date, whoami, wget) into a single binary. Because Kubernetes runs on Linux, BusyBox is a useful tool for debugging and troubleshooting inside a cluster.

##### Deployment
BusyBox can be deployed as a Pod using a Deployment manifest. Unlike application deployments, it is typically run with only one replica in the default namespace.

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: busybox-deployment
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: busybox
  template:
    metadata:
      labels:
        app: busybox
    spec:
      containers:
      - name: busybox
        image: busybox:latest
        command: [ "sleep", "3600" ]  # keeps container running
```

##### Key Commands
- **Create the Pod:** `kubectl apply -f busybox.yaml`  
- **Check the Pod:** `kubectl get pods`. Defaults to the default namespace.  
- **Get Pod IPs:** `kubectl get pods -n development -o wide`. Shows IPs of application Pods.  
- **Access BusyBox:** `kubectl exec -it <busybox-pod> -- /bin/sh`. Opens an interactive shell inside the Pod.  

##### Testing the Application
- Use `wget <pod-ip-address>:<port>` inside the BusyBox Pod to test connectivity to the application Pod’s IP address. Ensure the correct port is specified (e.g., 3000 instead of the default 80).  
- Successful requests save the response (e.g., as index.html).  

<small> Source: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>