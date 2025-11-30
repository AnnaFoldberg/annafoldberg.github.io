---
title: "Trackunit: Run Kubernetes Cluster Locally"
categories: [trackunit, kubernetes]
tags: [setup, final]
lang: en
locale: en
nav_order: 27
ref: run-k8s-cluster
---

##### Run Kubernetes Cluster Locally
1. Ensure Docker Desktop is running.  
2. Navigate to the `infra-core/k8s` directory.  
3. Create the Kubernetes cluster:
   ```bash
   kind create cluster --config kind-calico.yaml
   ```
4. Apply the calico CNI manifest:
   ```bash
   kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.3/manifests/calico.yaml
   ```
5. Install required Helm charts:
   ```bash
   helm install oauth2-proxy oauth2-proxy/oauth2-proxy --namespace api-gateway --create-namespace -f helm/oauth2-proxy/values.yaml  
   helm install kong kong/kong --namespace api-gateway --create-namespace -f helm/kong/values.yaml  
   helm install rabbitmq oci://registry-1.docker.io/cloudpirates/rabbitmq --namespace messaging --create-namespace -f helm/rabbitmq/values.yaml  
   ```
6. Apply foundational manifests:
   ```bash
   kubectl apply -f helm/oauth2-proxy/secret.yaml  
   kubectl apply -f helm/oauth2-proxy/network-policy.yaml  
   kubectl apply -f helm/kong/network-policy.yaml  
   kubectl apply -f helm/rabbitmq/network-policy.yaml  
   ```
7. Ensure all pods are running before proceeding:
   ```bash
   kubectl get pods -A --watch
   ```
8. Apply application manifests:
   ```bash
   kubectl apply -f graph-gateway  
   kubectl apply -f svc-analysis-orchestrator
   ```
   *If errors occur, simply run the two commands again. Sometimes manifests are applied out of order, so dependent resources may not exist the first time.*
9. Verify that all pods are running:
    ```bash
    kubectl get pods -A
    ```
10. Port-forward rabbitmq nodeport:
    ```bash
    kubectl -n messaging port-forward svc/rabbitmq 5672:5672
    ```
11. Open a new terminal and start cloud-provider-kind:
    ```bash
    sudo go/bin/cloud-provider-kind
    ```
    *This assigns an external IP to the Kong LoadBalancer service that allows it to be accessed from outside the Kubernetes cluster.*
12. (Optional) Open a new terminal and start tailscale funnel:
    ```bash
    tailscale funnel 5104
    ```
    *This is only required if the Kubernetes cluster needs to reach services outside Kubernetes.*

**Enforce TLS**
1. Uninstall existing Kong deployment:
   ```bash
   helm uninstall kong -n api-gateway
   ```
2. Create self-signed TLS certificate for graphql.local:
   ```bash
   openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout helm/kong/kong-tls.key -out helm/kong/kong-tls.crt -config helm/kong/san.cnf -extensions req_ext
   ```
3. Create TLS secret for Kong:
    ```bash
    kubectl create secret tls kong-ingress-tls --cert=helm/kong/kong-tls.crt --key=helm/kong/kong-tls.key -n api-gateway
    ```
4. Install Kong Helm chart:
   ```bash
   helm install kong kong/kong -n api-gateway -f helm/kong/values.ingress-tls.yaml
   ```
5. Apply ingress:
   ```bash
   kubectl apply -f helm/kong/ingress-graphql.yaml
   ```
6. Open Keychain Access → System → Certificates.  
7. Add certificate → set to Always Trust.

**Cleanup**
```bash
kind delete cluster
```