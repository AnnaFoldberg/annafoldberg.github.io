---
title: "Trackunit: Immagini precompilate"
categories: [trackunit]
tags: [docker, ci]
lang: it
locale: it
nav_order: 5
ref: prebuilt-images
---

Ogni servizio crea la propria immagine Docker nel proprio repository e la invia a un *container registry* (GHCR).  
Il repository **infra-core** scarica le immagini precompilate e le esegue tramite *docker-compose*.

**In ogni repository dell’app (CI):**  
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

**Nel docker-compose di infra-core:**  
```yaml
services:
  svc-analysis-orchestrator:
    image: ghcr.io/team-2-devs/svc-analysis-orchestrator:latest
  graph-gateway:
    image: ghcr.io/team-2-devs/graph-gateway:latest
```