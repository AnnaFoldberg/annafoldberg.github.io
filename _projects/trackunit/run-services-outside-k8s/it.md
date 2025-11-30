---
title: "Trackunit: Eseguire i Servizi al di Fuori del Cluster Kubernetes"
categories: [trackunit]
tags: [setup, final]
lang: it
locale: it
nav_order: 28
ref: run-services-outside-k8s
---

### Eseguire i Servizi al di Fuori del Cluster Kubernetes

**svc-ai-vision-adapter**  
1. Vai nella cartella `svc-ai-vision-adapter` ed esegui:  
   ```bash
   export GOOGLE_APPLICATION_CREDENTIALS=service-account.json                                  
   dotnet run
   ```

**tu-media-access-service**  
1. Nella root del repository, esegui:
   ```bash
   docker compose up -d
   ```

**tu-storage-service**  
1. Nella root del repository, esegui:
   ```bash
   docker compose up -d
   ```

**minIO**  
Accesso:  
- URL: http://localhost:9001  
- Username: `minioadmin`  
- Password: `minioadmin`  
Bucket predefinito: `trackunit-images` (crealo una volta se manca)

**tu-ingestion-service**
1. Vai in `src/Ingestion.Api` ed esegui:
   ```bash
   dotnet run
   ```

**svc-messaging-bridge**
1. Nella root del repository, esegui:
   ```bash
   dotnet run
   ```