---
title: "Creazione di Namespaces in Kubernetes"
categories: [kubernetes]
tags: [namespaces, manifests, kubectl]
lang: it
locale: it
nav_order: 40
ref: note-namespaces-kubernetes
---
I namespaces in Kubernetes vengono utilizzati per isolare e organizzare i workloads. Consentono di separare ambienti o applicazioni all’interno dello stesso cluster, ad esempio utilizzando un namespace per lo sviluppo e un altro per la produzione.

##### Namespaces predefiniti
Un nuovo cluster Kubernetes viene fornito con quattro namespaces predefiniti:

- default  
- kube-system  
- kube-public  
- kube-node-lease  

##### Manifests dei namespaces
I namespaces vengono definiti utilizzando YAML manifests. Più namespaces possono essere dichiarati in un singolo file separando i documenti con `---`.

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: development
---
apiVersion: v1
kind: Namespace
metadata:
  name: production
```

##### Comandi di gestione
- **Creare namespaces da un manifest:** `kubectl apply -f namespace.yaml`  
- **Visualizzare tutti i namespaces:** `kubectl get namespaces`  
- **Eliminare namespaces definiti in un manifest:** `kubectl delete -f namespace.yaml`  

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>