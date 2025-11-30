---
title: "Trackunit: Kør Services Udenfor Kubernetes Cluster"
categories: [trackunit]
tags: [setup, final]
lang: da
locale: da
nav_order: 28
ref: run-services-outside-k8s
---

##### Kør Services Udenfor Kubernetes Cluster
**svc-ai-vision-adapter**  
1. Gå til `svc-ai-vision-adapter`-mappen og kør:  
   ```bash
   export GOOGLE_APPLICATION_CREDENTIALS=service-account.json                                  
   dotnet run
   ```

**tu-media-access-service**  
1. Ved repo-root, kør:
   ```bash
   docker compose up -d
   ```

**tu-storage-service**  
1. Ved repo-root, kør:
   ```bash
   docker compose up -d
   ```

**minIO**  
Adgang:  
- URL: http://localhost:9001  
- Brugernavn: `minioadmin`  
- Adgangskode: `minioadmin`  
Standard-bucket: `trackunit-images` (opret den én gang, hvis den mangler)

**tu-ingestion-service**  
1. Gå til `src/Ingestion.Api` og kør:
   ```bash
   dotnet run
   ```

**svc-messaging-bridge**  
1. Ved repo-root, kør:
   ```bash
   dotnet run
   ```