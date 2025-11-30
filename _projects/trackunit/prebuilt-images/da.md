---
title: "Trackunit Prototype: Prebuilt Images"
categories: [trackunit]
tags: [docker, ci]
lang: da
locale: da
nav_order: 5
ref: prebuilt-images
---

Hver service bygger sit eget Docker-image i sit eget repository og uploader det til et container registry (GHCR).  
**infra-core**-repositoryet henter de prebuilt images og kører dem via *docker-compose*.

**I hvert app-repo (CI):**  
```yaml
      - name: Build & push image
        uses: docker/build-push-action@v6
        with:
          context: .
          file: ./Dockerfile
          push: true
          build-args: |
            GH_OWNER=${{ env.OWNER_LC }}
            GH_USER=${{ github.actor }}
            GH_TOKEN=${{ secrets.GITHUB_TOKEN }}
          tags: |
            ${{ env.IMAGE_REGISTRY }}/${{ env.OWNER_LC }}/${{ env.IMAGE_NAME }}:latest
```

**I infra-core docker-compose:**  
```yaml
services:
  svc-analysis-orchestrator:
    image: ghcr.io/team-2-devs/svc-analysis-orchestrator:latest
  graph-gateway:
    image: ghcr.io/team-2-devs/graph-gateway:latest
```