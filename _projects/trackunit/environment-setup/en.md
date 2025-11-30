---
title: "Trackunit: Environment Setup"
categories: [trackunit]
tags: [setup, infrastructure, final]
lang: en
locale: en
nav_order: 24
ref: environment-setup
---

1. Install Homebrew (if you don't already have it):
2. Install kind, kubectl, and helm:
   ```bash
   brew install kind
   brew install kubectl
   brew install helm
   ```
3. Add kong and oauth2-proxy helm charts to helm repo:
   ```bash
   helm repo add kong https://charts.konghq.com
   ```
   ```bash
   helm repo add oauth2-proxy https://oauth2-proxy.github.io/manifests
   ```
4. Install Go:
   ```bash
   brew install go
   ```
5. Install cloud-provider-kind:
   ```bash
   go install sigs.k8s.io/cloud-provider-kind@latest
   ```
6. Move cloud-provider-kind to /usr/local/bin for global use:
   ```bash
   sudo mv ~/go/bin/cloud-provider-kind /usr/local/bin/cloud-provider-kind
   ```