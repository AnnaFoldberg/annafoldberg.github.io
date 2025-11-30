---
title: "Trackunit Conventions: YAML Filenames"
categories: [trackunit]
tags: [yaml, naming-conventions]
lang: en
locale: en
nav_order: 4
ref: yaml-filenames
---

`.yml` and `.yaml` are the same file format. The project files follow this naming convention:

- **Docker/Compose:** `.yml` as it matches Docker’s docs, Compose CLI defaults, and historical usage.
- **Kubernetes/Helm:** `.yaml` as it matches the YAML specification and official K8s documentation.
- **GitHub Actions:** When GitHub Actions launched, the `.yml` form was already dominant in Docker/CI contexts and has remained the de facto standard.