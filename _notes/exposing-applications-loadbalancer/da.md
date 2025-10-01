---
title: "Eksponering af Applikationer med en LoadBalancer"
categories: [kubernetes]
tags: [loadbalancer, services]
lang: da
locale: da
nav_order: 45
ref: note-exposing-applications-loadbalancer
---
Kubernetes Services fungerer som load balancers, der dirigerer trafik til Pods. En LoadBalancer Service leverer en offentlig, statisk IP-adresse, hvilket gør applikationer tilgængelige fra internettet.

- **Public:** alle kan få adgang udefra clusteret.  
- **Static:** forbliver den samme, selv når Pods genskabes, og deres IP’er ændrer sig.  

##### Service-manifest
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

##### Gennemgang af manifest
- **selector:** Dirigerer trafik til Pods med label app: pod-info. Disse labels skal matche dem i Deployment-spec’en.  
- **ports:** Eksponerer port 80 på servicen og sender trafik videre til port 3000 i containeren.  
- **type:** LoadBalancer. En af tre Service-typer (ClusterIP, NodePort, LoadBalancer).  

##### Administrationskommandoer
- **Kør tunnel (kun Minikube):** `minikube tunnel`. Giver ekstern IP på lokale clusters.  
- **Opret Service:** `kubectl apply -f service.yaml`  
- **Tjek Services:** `kubectl get services -n development`. Viser både interne og eksterne IP’er.  

##### Noter
- I Minikube er “external IP” faktisk 127.0.0.1, da det kører lokalt.  
- I cloud-miljøer (GCP, AWS, Azure) tildeles en reel offentlig IP-adresse.  

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>