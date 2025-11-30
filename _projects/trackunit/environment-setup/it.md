---
title: "Trackunit: Setup Ambiente"
categories: [trackunit]
tags: [setup, infrastructure, final]
lang: it
locale: it
nav_order: 24
ref: environment-setup
---

1. Installa Homebrew (se non lo hai già):
2. Installa kind, kubectl e helm:
   ```bash
   brew install kind
   brew install kubectl
   brew install helm
   ```
3. Aggiungi i chart helm di kong e oauth2-proxy al repo helm:
   ```bash
   helm repo add kong https://charts.konghq.com
   ```
   ```bash
   helm repo add oauth2-proxy https://oauth2-proxy.github.io/manifests
   ```
4. Installa Go:
   ```bash
   brew install go
   ```
5. Installa cloud-provider-kind:
   ```bash
   go install sigs.k8s.io/cloud-provider-kind@latest
   ```
6. Sposta cloud-provider-kind in /usr/local/bin per l’uso globale:
   ```bash
   sudo mv ~/go/bin/cloud-provider-kind /usr/local/bin/cloud-provider-kind
   ```