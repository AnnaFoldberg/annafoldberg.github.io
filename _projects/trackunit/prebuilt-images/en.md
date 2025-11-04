---
title: "Trackunit: Prebuilt Images"
categories: [trackunit]
tags: [docker, ci]
lang: en
locale: en
nav_order: 5
ref: prebuilt-images
---

Each service builds its own Docker image in its own repository and pushes it to a container registry (GHCR).  
The **infra-core** repository pulls the prebuilt images and runs them via *docker-compose*.

**In each app repo (CI):**  
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

**In infra-core docker-compose:**  
```yaml
services:
  svc-analysis-orchestrator:
    image: ghcr.io/team-2-devs/svc-analysis-orchestrator:latest
  graph-gateway:
    image: ghcr.io/team-2-devs/graph-gateway:latest
```