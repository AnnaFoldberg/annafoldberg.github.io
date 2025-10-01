---
title: "Deploying an Application with a Deployment"
categories: [kubernetes]
tags: [deployments, pods, replicas]
lang: en
locale: en
nav_order: 41
ref: note-deploying-application-deployment
---
Kubernetes is designed to make applications highly available by running multiple replicas at the same time. If one replica fails, others remain to accept traffic. Pods are the Kubernetes resource that run applications and microservices. A Deployment ensures that Pods are organized, replicated, and kept running.

##### Deployment Manifest
```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pod-info-deployment
  namespace: development
  labels:
    app: pod-info
spec:
  replicas: 3
  selector:
    matchLabels:
      app: pod-info
  template:
    metadata:
      labels:
        app: pod-info
    spec:
      containers:
      - name: pod-info-container
        image: kimschles/pod-info-app:latest
        ports:
        - containerPort: 3000
        env:
          - name: POD_NAME
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: POD_NAMESPACE
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
          - name: POD_IP
            valueFrom:
              fieldRef:
                fieldPath: status.podIP
```

##### Manifest Breakdown
- **apiVersion:** apps/v1, the API group used for deployments.  
- **kind:** Deployment, specifies the Kubernetes object.  
- **metadata:** Includes the deployment name, namespace, and labels for the Pods.  
- **spec.replicas:** Number of Pod replicas to maintain (set to 3 for high availability).  
- **containers:** Defines the container to run, its image (kimschles/pod-info-app:latest), exposed port (3000), and environment variables (POD_NAME, POD_NAMESPACE, POD_IP).  

##### Management Commands
- **Ensure the target namespace exists:** `kubectl apply -f namespace.yaml`  
- **Create the deployment:** `kubectl apply -f deployment.yaml`  
- **List deployments in the development namespace:** `kubectl get deployments -n development`  
- **View the Pods created by the deployment:** `kubectl get pods -n development`  
- **Delete a Pod and watch the deployment automatically create a replacement:** `kubectl delete pod <pod-name> -n development`  

<small> Source: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>