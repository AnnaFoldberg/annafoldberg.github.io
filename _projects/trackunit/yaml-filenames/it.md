---
title: "Trackunit Conventions: Nomi dei file YAML"
categories: [trackunit]
tags: [yaml, naming-conventions]
lang: it
locale: it
nav_order: 4
ref: yaml-filenames
---

`.yml` e `.yaml` sono lo stesso formato di file. I file del progetto seguono questa convenzione di denominazione:

- **Docker/Compose:** `.yml`, poiché corrisponde alla documentazione di Docker, ai valori predefiniti di Compose CLI e all’uso storico.
- **Kubernetes/Helm:** `.yaml`, poiché segue la specifica YAML e la documentazione ufficiale di Kubernetes.
- **GitHub Actions:** Quando è stato lanciato GitHub Actions, la forma `.yml` era già dominante nei contesti Docker/CI ed è rimasta lo standard de facto.