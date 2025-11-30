---
title: "Trackunit Prototype: YAML-filnavne"
categories: [trackunit]
tags: [naming-conventions, yaml]
lang: da
locale: da
nav_order: 4
ref: yaml-filenames
---

`.yml` og `.yaml` er det samme filformat. Projektfilerne følger denne navngivningskonvention:

- **Docker/Compose:** `.yml`, da det matcher Dockers dokumentation, Compose CLI-standard og historisk brug.
- **Kubernetes/Helm:** `.yaml`, da det matcher YAML-specifikationen og den officielle Kubernetes-dokumentation.
- **GitHub Actions:** Da GitHub Actions blev lanceret, var `.yml` allerede dominerende i Docker/CI-sammenhænge og er forblevet de facto-standarden.