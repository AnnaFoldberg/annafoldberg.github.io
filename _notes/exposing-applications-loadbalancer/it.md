---
title: "Esposizione delle Applicazioni con un LoadBalancer"
categories: [kubernetes]
tags: [loadbalancer, services]
lang: it
locale: it
nav_order: 45
ref: note-exposing-applications-loadbalancer
---
I Services di Kubernetes agiscono come load balancers, indirizzando il traffico ai Pods. Un Service di tipo LoadBalancer fornisce un indirizzo IP pubblico e statico, rendendo le applicazioni accessibili da internet.

- **Public:** chiunque può accedervi dall’esterno del cluster.  
- **Static:** rimane lo stesso anche quando i Pods vengono ricreati e i loro IP cambiano.  

##### Manifest del Service
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

##### Spiegazione del manifest
- **selector:** Instrada il traffico ai Pods etichettati app: pod-info. Queste labels devono corrispondere a quelle nello spec del Deployment.  
- **ports:** Espone la porta 80 del Service e inoltra il traffico alla porta 3000 all’interno del container.  
- **type:** LoadBalancer. Uno dei tre tipi di Service (ClusterIP, NodePort, LoadBalancer).  

##### Comandi di gestione
- **Eseguire il tunnel (solo Minikube):** `minikube tunnel`. Fornisce IP esterno nei cluster locali.  
- **Creare il Service:** `kubectl apply -f service.yaml`  
- **Verificare i Services:** `kubectl get services -n development`. Elenca sia gli IP interni che esterni.  

##### Note
- In Minikube, l’“external IP” è in realtà 127.0.0.1, poiché gira in locale.  
- Negli ambienti cloud (GCP, AWS, Azure) viene assegnato un vero indirizzo IP pubblico.  

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>