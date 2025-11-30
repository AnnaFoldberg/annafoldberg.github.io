---
title: "Trackunit: Run Services Outside Kubernetes Cluster"
categories: [trackunit]
tags: [setup, final]
lang: en
locale: en
nav_order: 28
ref: run-services-outside-k8s
---

### Run Services Outside Kubernetes Cluster

**svc-ai-vision-adapter**  
1. Go to the `svc-ai-vision-adapter` folder and run:  
   ```bash
   export GOOGLE_APPLICATION_CREDENTIALS=service-account.json                                  
   dotnet run
   ```

**tu-media-access-service**  
1. At repo root, run:
   ```bash
   docker compose up -d
   ```

**tu-storage-service**  
1. At repo root, run:
   ```bash
   docker compose up -d
   ```

**minIO**  
Access:  
- URL: http://localhost:9001  
- Username: `minioadmin`  
- Password: `minioadmin`  
Default bucket: `trackunit-images` (create it once if missing)

**tu-ingestion-service**
1. Go to `src/Ingestion.Api` and run:
   ```bash
   dotnet run
   ```

**svc-messaging-bridge**
1. At repo root, run:
   ```bash
   dotnet run
   ```