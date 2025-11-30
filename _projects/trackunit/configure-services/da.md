---
title: "Trackunit: Configure Services"
categories: [trackunit, microservices]
tags: [configuration, infrastructure, final]
lang: da
locale: da
nav_order: 26
ref: configure-services
---

Projektet består både af services, der kører i et Kubernetes-cluster, og services, der kører uden for clusteret, enten via Docker Compose eller direkte på hosten. For at kommunikere på tværs af disse miljøer kræver nogle eksterne services justerede konfigurationer i forhold til det, der ligger i deres respektive repositories.

##### Configure Services

**infra-core/k8s/graph-gateway/secret.yaml**  
1. Vælg værdi i stedet for `changeme` og brug den konsekvent i alle fremtidige `.env`-filer:
   ```yaml
   INTERNAL_AUTH_API_KEY: changeme  
   INGESTION_BASE_URL: "https://<address>.ts.net" # insert funnel address
   ```

**svc-messaging-bridge**  
1. Opret `.env` i repo-roden:
   ```yaml
   RABBIT_HOST=localhost
   RABBIT_USER=graph
   RABBIT_PASS="<rabbit-password>" # insert rabbitmq password
   RABBIT_PORT=5672
   KAFKA_BROKERS=100.106.102.107:9092 # insert personal tailscale IP
   ```
2. Opret GitHub-token:  
   a. GitHub → Profile → Settings  
   b. Developer settings → Personal access token → Tokens (classic)  
   c. Generate new token → Generate new token (classic)  
   d. Giv tokenet et navn og tjek read:packages  
   e. Klik generate og kopier token  
3. Kør:
   ```bash
   dotnet nuget add source "https://nuget.pkg.github.com/team-2-devs/index.json" --name "github" --username <github-username> --password <token> --store-password-in-clear-text
   ```
4. Kør:
   ```bash
   dotnet restore
   ```

**tu-media-access-service**  
1. I `Dockerfile`:  
   ```yaml
   ENV ASPNETCORE_URLS=http://+:9080 # previously not defined

   EXPOSE 9080 # previously 8080
   ```
2. I `docker-compose.yml`:
   ```yaml
   ports:
   - "5136:9080" # previously 5136:8080
   ```
3. I `appsettings.Development.json`:
   ```yaml
   "BaseUrl": "http://localhost:9080" # previously :8080
   ```
4. Opret `.env` i repo-roden:
   ```yaml
   INTERNAL_AUTH_API_KEY=changeme
   STORAGE_BASE_URL=http://host.docker.internal:9080 # previously :8080
   ```

**tu-storage-service**  
1. I `Dockerfile`:
   ```yaml
   ENV ASPNETCORE_URLS=http://+:9080 # previously 8080

   EXPOSE 9080 # previously 8080
   ```
2. I `docker-compose.yml`:
   ```yaml
   ports:
   - "9080:9080" # previously 8080:8080
   ```
3. Opret `.env` i repo-roden:
   ```yaml
   MINIO_ROOT_USER=minioadmin
   MINIO_ROOT_PASSWORD=minioadmin

   INTERNAL_AUTH_API_KEY=changeme
   ```

**tu-ingestion-service**  
1. I `Dockerfile`:
   ```yaml
   ENV ASPNETCORE_URLS=http://+:9080 # previously 8080

   EXPOSE 9080 # previously 8080
   ```
2. I `docker-compose.yml`:
   ```yaml
   ports:
   - "9090:9080" # previously 8090:8080
   ```
3. I `appsettings.Development.json`:
   ```yaml
   "Messaging": {
      "Producer": "ingestion",
      "Redpanda": {
         "BootstrapServers": "100.106.102.107:9092" # insert personal tailscale IP 
   }

   "Storage": {
      "BaseUrl": "http://localhost:9080", # previously :8080
      "InternalAccess": "changeme" # previously placeholder
   }
   ```
4. Opret `.env` i repo-roden:
   ```yaml
   REDPANDA_BOOTSTRAP_SERVERS=100.106.102.107:9092 # insert personal tailscale IP
   INTERNAL_AUTH_API_KEY=changeme
   STORAGE_BASE_URL=http://host.docker.internal:9080 # previously :8080
   ```
5. I `Ingestion.Api.csproj`, tilføj DotNetEnv package:
   ```csharp
   <PackageReference Include="DotNetEnv" Version="3.1.1" />
   ```
6. I starten af `Program.cs`, tilføj:
   ```csharp
   using DotNetEnv;

   Env.Load();
   ```
7. I repo-roden, kør:
   ```bash
   mkdir .local/ingestion/
   ```

**svc-ai-vision-adapter**  
1. I `launchsettings.json`:
   ```yaml
   "environmentVariables": {
      "ASPNETCORE_HTTPS_PORTS": "9081", # previously 8081
      "ASPNETCORE_HTTP_PORTS": "9080" # previously 8080
   },
   ```
2. I `appsettings.json`:
   ```yaml
   "Kafka": {
      "Consumer": {
         "BootstrapServers": "100.106.102.107:9092", # insert personal tailscale IP
         "GroupId": "svc-ai-vision-adapter",
         "Topic": "tu.images.uploaded",
         "EnableAutoCommit": true
      },
      "Producer": {
         "BootstrapServers": "100.106.102.107:9092", # insert personal tailscale IP
         "Topic": "tu.recognition.completed",
         "Acks": "All",
         "MessageSendMaxRetries": 3
      }
   ```
3. I `Program.cs` (push ikke den rigtige X-Internal-Access værdi):
   ```csharp
   builder.Services.AddHttpClient<IImageUrlFetcher, HttpImageUrlFetcher>(client =>
   {
      client.BaseAddress = new Uri("http://localhost:5136");
      client.DefaultRequestHeaders.Add("X-Internal-Access", "changeme");
   });
   ```
4. I `.gitignore`:
   ```yaml
   service-account.json
   ```
5. Kopiér secret sendt via https://eu.onetimesecret.com/ og indsæt i kommandoen nedenfor.
6. Gå til `svc-ai-vision-adapter`-mappen og kør:
   ```bash
   echo '<secret>' \  | base64 -d > service-account.json
   ```