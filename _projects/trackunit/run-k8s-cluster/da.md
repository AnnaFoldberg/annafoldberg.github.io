---
title: "Trackunit: Kør Kubernetes Cluster Lokalt"
categories: [trackunit, kubernetes]
tags: [setup, final]
lang: da
locale: da
nav_order: 27
ref: run-k8s-cluster
---

##### Kør Kubernetes Cluster Lokalt
1. Sørg for, at Docker Desktop kører.  
2. Navigér til `infra-core/k8s`-mappen.  
3. Opret Kubernetes-clusteret:
   ```bash
   kind create cluster --config kind-calico.yaml
   ```
4. Anvend calico CNI-manifestet:
   ```bash
   kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.3/manifests/calico.yaml
   ```
5. Installér de nødvendige Helm charts:
   ```bash
   helm install oauth2-proxy oauth2-proxy/oauth2-proxy --namespace api-gateway --create-namespace -f helm/oauth2-proxy/values.yaml  
   helm install kong kong/kong --namespace api-gateway --create-namespace -f helm/kong/values.yaml  
   helm install rabbitmq oci://registry-1.docker.io/cloudpirates/rabbitmq --namespace messaging --create-namespace -f helm/rabbitmq/values.yaml  
   ```
6. Anvend de grundlæggende manifests:
   ```bash
   kubectl apply -f helm/oauth2-proxy/secret.yaml  
   kubectl apply -f helm/oauth2-proxy/network-policy.yaml  
   kubectl apply -f helm/kong/network-policy.yaml  
   kubectl apply -f helm/rabbitmq/network-policy.yaml  
   ```
7. Sørg for, at alle pods kører, før du går videre:
   ```bash
   kubectl get pods -A --watch
   ```
8. Anvend applikations-manifests:
   ```bash
   kubectl apply -f graph-gateway  
   kubectl apply -f svc-analysis-orchestrator
   ```
   *If errors occur, simply run the two commands again. Sometimes manifests are applied out of order, so dependent resources may not exist the first time.*
9. Verificér, at alle pods kører:
    ```bash
    kubectl get pods -A
    ```
10. Port-forward RabbitMQ nodeport:
    ```bash
    kubectl -n messaging port-forward svc/rabbitmq 5672:5672
    ```
11. Åbn et nyt terminalvindue og start cloud-provider-kind:
    ```bash
    sudo go/bin/cloud-provider-kind
    ```
    *Dette tildeler en ekstern IP til Kong LoadBalancer-servicen, så den kan tilgås uden for Kubernetes-clusteret.*
12. (Valgfrit) Åbn et nyt terminalvindue og start tailscale funnel:
    ```bash
    tailscale funnel 5104
    ```
    *Dette er kun nødvendigt, hvis Kubernetes-clusteret skal kunne nå services uden for Kubernetes.*

**Enforce TLS**
1. Afinstallér den eksisterende Kong-deployment:
   ```bash
   helm uninstall kong -n api-gateway
   ```
2. Opret et self-signed TLS-certifikat til graphql.local:
   ```bash
   openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout helm/kong/kong-tls.key -out helm/kong/kong-tls.crt -config helm/kong/san.cnf -extensions req_ext
   ```
3. Opret TLS-secret til Kong:
    ```bash
    kubectl create secret tls kong-ingress-tls --cert=helm/kong/kong-tls.crt --key=helm/kong/kong-tls.key -n api-gateway
    ```
4. Installér Kong Helm chart med `values.ingress-tls.yaml` for at aktivere Ingress Controller-mode og TLS:
   ```bash
   helm install kong kong/kong -n api-gateway -f helm/kong/values.ingress-tls.yaml
   ```
5. Anvend ingress:
   ```bash
   kubectl apply -f helm/kong/ingress-graphql.yaml
   ```
6. Åbn `Keychain Access`.
7. Vælg `System` → `Certificates`.
8. Træk `kong-tls.crt` ind i certifikatslisten.
9. Dobbeltklik på det importerede certifikat → udvid `Trust` → sæt `When using this certificate` til `Always Trust`.

**Cleanup:**  
Fjern kind-clusteret:
```bash
kind delete cluster
```