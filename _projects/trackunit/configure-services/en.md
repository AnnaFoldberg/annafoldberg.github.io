---
title: "Trackunit: Configure Services"
categories: [trackunit, microservices]
tags: [configuration, infrastructure, final]
lang: en
locale: en
nav_order: 26
ref: configure-services
---

The project consists of services running inside a Kubernetes cluster, as well as services running outside the cluster using either Docker Compose or directly on the host. To enable communication across these environments, certain external services require adjusted configurations relative to what exists in their repositories.

##### Configure Services

**infra-core/k8s/graph-gateway/secret.yaml**  
1. Choose a value to replace `changeme` and use it consistently across all future `.env` files:
   ```yaml
   INTERNAL_AUTH_API_KEY: changeme  
   INGESTION_BASE_URL: "https://<address>.ts.net" # insert funnel address
   ```

**svc-messaging-bridge**  
1. Create `.env` at repository root:
   ```yaml
   RABBIT_HOST=localhost
   RABBIT_USER=graph
   RABBIT_PASS="<rabbit-password>" # insert rabbitmq password
   RABBIT_PORT=5672
   KAFKA_BROKERS=100.106.102.107:9092 # insert personal tailscale IP
   ```
2. Create GitHub token:  
   a. GitHub → Profile → Settings  
   b. Developer settings → Personal access tokens → Tokens (classic)  
   c. Generate new token → Generate new token (classic)  
   d. Name the token and check *read:packages*  
   e. Generate and copy token  
3. Run:
   ```bash
   dotnet nuget add source "https://nuget.pkg.github.com/team-2-devs/index.json" --name "github" --username <github-username> --password <token> --store-password-in-clear-text
   ```
4. Run:
   ```bash
   dotnet restore
   ```

**tu-media-access-service**  
1. In `Dockerfile`:  
   ```yaml
   ENV ASPNETCORE_URLS=http://+:9080 # previously not defined

   EXPOSE 9080 # previously 8080
   ```
2. In `docker-compose.yml`:
   ```yaml
   ports:
   - "5136:9080" # previously 5136:8080
   ```
3. In `appsettings.Development.json`:
   ```yaml
   "BaseUrl": "http://localhost:9080" # previously :8080
   ```
4. Create `.env` at repo root:
   ```yaml
   INTERNAL_AUTH_API_KEY=changeme
   STORAGE_BASE_URL=http://host.docker.internal:9080 # previously :8080
   ```

**tu-storage-service**  
1. In `Dockerfile`:
   ```yaml
   ENV ASPNETCORE_URLS=http://+:9080 # previously 8080

   EXPOSE 9080 # previously 8080
   ```
2. In `docker-compose.yml`:
   ```yaml
   ports:
   - "9080:9080" # previously 8080:8080
   ```
3. Create `.env`:
   ```yaml
   MINIO_ROOT_USER=minioadmin
   MINIO_ROOT_PASSWORD=minioadmin

   INTERNAL_AUTH_API_KEY=changeme
   ```

**tu-ingestion-service**  
1. In `Dockerfile`:
   ```yaml
   ENV ASPNETCORE_URLS=http://+:9080 # previously 8080

   EXPOSE 9080 # previously 8080
   ```
2. In `docker-compose.yml`:
   ```yaml
   ports:
   - "9090:9080" # previously 8090:8080
   ```
3. In `appsettings.Development.json`:
   ```yaml
   "Messaging": {
      "Producer": "ingestion",
      "Redpanda": {
         "BootstrapServers": "100.106.102.107:9092" # insert personal tailscale IP 
   }

   "Storage": {
      "BaseUrl": "http://localhost:9080",
      "InternalAccess": "changeme"
   }
   ```
4. Create `.env`:
   ```yaml
   REDPANDA_BOOTSTRAP_SERVERS=100.106.102.107:9092 # insert personal tailscale IP
   INTERNAL_AUTH_API_KEY=changeme
   STORAGE_BASE_URL=http://host.docker.internal:9080
   ```
5. In `Ingestion.Api.csproj`, add DotNetEnv:
   ```csharp
   <PackageReference Include="DotNetEnv" Version="3.1.1" />
   ```
6. At the beginning of `Program.cs`:
   ```csharp
   using DotNetEnv;

   Env.Load();
   ```
7. At repository root, run:
   ```bash
   mkdir .local/ingestion/
   ```

**svc-ai-vision-adapter**  
1. In `launchsettings.json`:
   ```yaml
   "environmentVariables": {
      "ASPNETCORE_HTTPS_PORTS": "9081",
      "ASPNETCORE_HTTP_PORTS": "9080"
   },
   ```
2. In `appsettings.json`:
   ```yaml
   "Kafka": {
      "Consumer": {
         "BootstrapServers": "100.106.102.107:9092", # personal tailscale IP
         "GroupId": "svc-ai-vision-adapter",
         "Topic": "tu.images.uploaded",
         "EnableAutoCommit": true
      },
      "Producer": {
         "BootstrapServers": "100.106.102.107:9092",
         "Topic": "tu.recognition.completed",
         "Acks": "All",
         "MessageSendMaxRetries": 3
      }
   ```
3. In `Program.cs` (do NOT commit real X-Internal-Access value):
   ```csharp
   builder.Services.AddHttpClient<IImageUrlFetcher, HttpImageUrlFetcher>(client =>
   {
      client.BaseAddress = new Uri("http://localhost:5136");
      client.DefaultRequestHeaders.Add("X-Internal-Access", "changeme");
   });
   ```
4. In `.gitignore`:
   ```yaml
   service-account.json
   ```
5. Copy secret sent via https://eu.onetimesecret.com/ and insert into command below.
6. Inside `svc-ai-vision-adapter` folder, run:
   ```bash
   echo '<secret>' \ | base64 -d > service-account.json
   ```