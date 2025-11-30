---
title: "Trackunit: Eseguire il Cluster Kubernetes in Locale"
categories: [trackunit, kubernetes]
tags: [setup, final]
lang: it
locale: it
nav_order: 27
ref: run-k8s-cluster
---

##### Eseguire il Cluster Kubernetes in Locale
1. Assicurati che Docker Desktop sia in esecuzione.  
2. Vai nella directory `infra-core/k8s`.  
3. Crea il cluster Kubernetes:
   ```bash
   kind create cluster --config kind-calico.yaml
   ```
4. Applica il manifest CNI di calico:
   ```bash
   kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.3/manifests/calico.yaml
   ```
5. Installa i chart Helm richiesti:
   ```bash
   helm install oauth2-proxy oauth2-proxy/oauth2-proxy --namespace api-gateway --create-namespace -f helm/oauth2-proxy/values.yaml  
   helm install kong kong/kong --namespace api-gateway --create-namespace -f helm/kong/values.yaml  
   helm install rabbitmq oci://registry-1.docker.io/cloudpirates/rabbitmq --namespace messaging --create-namespace -f helm/rabbitmq/values.yaml  
   ```
6. Applica i manifest di base:
   ```bash
   kubectl apply -f helm/oauth2-proxy/secret.yaml  
   kubectl apply -f helm/oauth2-proxy/network-policy.yaml  
   kubectl apply -f helm/kong/network-policy.yaml  
   kubectl apply -f helm/rabbitmq/network-policy.yaml  
   ```
7. Verifica che tutti i pod siano in esecuzione prima di procedere:
   ```bash
   kubectl get pods -A --watch
   ```
8. Applica i manifest applicativi:
   ```bash
   kubectl apply -f graph-gateway  
   kubectl apply -f svc-analysis-orchestrator
   ```
   *Se si verificano errori, riesegui semplicemente i due comandi. A volte i manifest vengono applicati in ordine errato, quindi le risorse dipendenti potrebbero non esistere al primo tentativo.*
9. Verifica che tutti i pod siano in esecuzione:
    ```bash
    kubectl get pods -A
    ```
10. Esegui il port-forward del nodeport di rabbitmq:
    ```bash
    kubectl -n messaging port-forward svc/rabbitmq 5672:5672
    ```
11. Apri un nuovo terminale e avvia cloud-provider-kind:
    ```bash
    sudo go/bin/cloud-provider-kind
    ```
    *Questo assegna un IP esterno al servizio Kong LoadBalancer, permettendone l’accesso dall’esterno del cluster Kubernetes.*
12. (Opzionale) Apri un nuovo terminale e avvia un tailscale funnel:
    ```bash
    tailscale funnel 5104
    ```
    *Questo è necessario solo se il cluster Kubernetes deve raggiungere servizi al di fuori di Kubernetes.*

**Enforce TLS**
1. Disinstalla la deployment esistente di Kong:
   ```bash
   helm uninstall kong -n api-gateway
   ```
2. Crea un certificato TLS self-signed per `graphql.local`:
   ```bash
   openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout helm/kong/kong-tls.key -out helm/kong/kong-tls.crt -config helm/kong/san.cnf -extensions req_ext
   ```
3. Crea il secret TLS per Kong:
    ```bash
    kubectl create secret tls kong-ingress-tls --cert=helm/kong/kong-tls.crt --key=helm/kong/kong-tls.key -n api-gateway
    ```
4. Installa il chart Helm di Kong con `values.ingress-tls.yaml` per abilitare la modalità Ingress Controller e TLS:
   ```bash
   helm install kong kong/kong -n api-gateway -f helm/kong/values.ingress-tls.yaml
   ```
5. Applica l’ingress:
   ```bash
   kubectl apply -f helm/kong/ingress-graphql.yaml
   ```
6. Apri `Keychain Access`.  
7. Seleziona `System` → `Certificates`.  
8. Trascina `kong-tls.crt` nell’elenco dei certificati.  
9. Fai doppio clic sul certificato importato → espandi `Trust` → imposta `When using this certificate` su `Always Trust`.

**Cleanup:**  
Rimuovi il cluster kind:
```bash
kind delete cluster
```