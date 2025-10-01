---
title: "Exposing Applications with a LoadBalancer"
categories: [kubernetes]
tags: [loadbalancer, services]
lang: en
locale: en
nav_order: 45
ref: note-exposing-applications-loadbalancer
---
Kubernetes Services act as load balancers, directing traffic to Pods. A LoadBalancer Service provides a public, static IP address, making applications accessible from the internet.

- **Public:** anyone can access it from outside the cluster.  
- **Static:** remains the same even as Pods are recreated and their IPs change.  

##### Service Manifest
```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: demo-service
  namespace: development
spec:
  selector:
    app: pod-info
  ports:
    - port: 80         # service port
      targetPort: 3000 # container port
  type: LoadBalancer
```

##### Manifest Breakdown
- **selector:** Routes traffic to Pods labeled app: pod-info. These labels must match those in the Deployment spec.  
- **ports:** Exposes port 80 on the Service and directs traffic to port 3000 inside the container.  
- **type:** LoadBalancer. One of three Service types (ClusterIP, NodePort, LoadBalancer).  

##### Management Commands
- **Run tunnel (Minikube only):** `minikube tunnel`. Provides external IP on local clusters.  
- **Create the Service:** `kubectl apply -f service.yaml`  
- **Check Services:** `kubectl get services -n development`. Lists both internal and external IPs.  

##### Notes
- In Minikube, the “external IP” is actually 127.0.0.1, since it runs locally.  
- In cloud environments (GCP, AWS, Azure), a real public IP address is assigned.  

<small> Source: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>