---
title: "Trackunit: Environment Setup"
categories: [trackunit]
tags: [setup, infrastructure, final]
lang: da
locale: da
nav_order: 24
ref: environment-setup
---

1. Installér Homebrew (hvis du ikke allerede har det):
2. Installér kind, kubectl og helm:
   ```bash
   brew install kind
   brew install kubectl
   brew install helm
   ```
3. Tilføj helm charts for kong og oauth2-proxy til helm repo:
   ```bash
   helm repo add kong https://charts.konghq.com
   ```
   ```bash
   helm repo add oauth2-proxy https://oauth2-proxy.github.io/manifests
   ```
4. Installér Go:
   ```bash
   brew install go
   ```
5. Installér cloud-provider-kind:
   ```bash
   go install sigs.k8s.io/cloud-provider-kind@latest
   ```
6. Flyt cloud-provider-kind til /usr/local/bin for global anvendelse:
   ```bash
   sudo mv ~/go/bin/cloud-provider-kind /usr/local/bin/cloud-provider-kind
   ```