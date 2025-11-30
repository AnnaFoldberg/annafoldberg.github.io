---
title: "Trackunit: Configurare i Servizi"
categories: [trackunit, microservices]
tags: [configuration, infrastructure, final]
lang: it
locale: it
nav_order: 26
ref: configure-services
---

Il progetto include servizi che girano dentro un cluster Kubernetes e servizi che girano al di fuori del cluster, tramite Docker Compose o direttamente sull’host. Per permettere la comunicazione tra questi ambienti, alcuni servizi esterni richiedono configurazioni modificate rispetto ai loro repository originali.

##### Configurare i Servizi

**infra-core/k8s/graph-gateway/secret.yaml**  
1. Scegli un valore al posto di `changeme` e usalo in modo coerente in tutti i futuri file `.env`:
   ```yaml
   INTERNAL_AUTH_API_KEY: changeme  
   INGESTION_BASE_URL: "https://<address>.ts.net" # inserire indirizzo funnel
   ```

**svc-messaging-bridge**  
1. Crea `.env` nella root del repository:
   ```yaml
   RABBIT_HOST=localhost
   RABBIT_USER=graph
   RABBIT_PASS="<rabbit-password>"
   RABBIT_PORT=5672
   KAFKA_BROKERS=100.106.102.107:9092 # IP Tailscale personale
   ```
2. Crea un token GitHub:  
   a. GitHub → Profilo → Settings  
   b. Developer settings → Personal access tokens → Tokens (classic)  
   c. Generate new token  
   d. Assegna un nome e seleziona *read:packages*  
   e. Genera e copia il token  
3. Esegui:
   ```bash
   dotnet nuget add source "https://nuget.pkg.github.com/team-2-devs/index.json" --name "github" --username <github-username> --password <token> --store-password-in-clear-text
   ```
4. Esegui:
   ```bash
   dotnet restore
   ```

**tu-media-access-service**  
1. In `Dockerfile`:
   ```yaml
   ENV ASPNETCORE_URLS=http://+:9080
   EXPOSE 9080
   ```
2. In `docker-compose.yml`:
   ```yaml
   ports:
   - "5136:9080"
   ```
3. In `appsettings.Development.json`:
   ```yaml
   "BaseUrl": "http://localhost:9080"
   ```
4. Crea `.env`:
   ```yaml
   INTERNAL_AUTH_API_KEY=changeme
   STORAGE_BASE_URL=http://host.docker.internal:9080
   ```

**tu-storage-service**  
1. In `Dockerfile`:
   ```yaml
   ENV ASPNETCORE_URLS=http://+:9080
   EXPOSE 9080
   ```
2. In `docker-compose.yml`:
   ```yaml
   ports:
   - "9080:9080"
   ```
3. Crea `.env`:
   ```yaml
   MINIO_ROOT_USER=minioadmin
   MINIO_ROOT_PASSWORD=minioadmin
   INTERNAL_AUTH_API_KEY=changeme
   ```

**tu-ingestion-service**  
1. In `Dockerfile`:
   ```yaml
   ENV ASPNETCORE_URLS=http://+:9080
   EXPOSE 9080
   ```
2. In `docker-compose.yml`:
   ```yaml
   ports:
   - "9090:9080"
   ```
3. In `appsettings.Development.json`:
   ```yaml
   "Messaging": {
      "Producer": "ingestion",
      "Redpanda": {
         "BootstrapServers": "100.106.102.107:9092"
   }

   "Storage": {
      "BaseUrl": "http://localhost:9080",
      "InternalAccess": "changeme"
   }
   ```
4. Crea `.env`:
   ```yaml
   REDPANDA_BOOTSTRAP_SERVERS=100.106.102.107:9092
   INTERNAL_AUTH_API_KEY=changeme
   STORAGE_BASE_URL=http://host.docker.internal:9080
   ```
5. In `Ingestion.Api.csproj`, aggiungi DotNetEnv:
   ```csharp
   <PackageReference Include="DotNetEnv" Version="3.1.1" />
   ```
6. All’inizio di `Program.cs`:
   ```csharp
   using DotNetEnv;

   Env.Load();
   ```
7. Nella root del repository, esegui:
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
         "BootstrapServers": "100.106.102.107:9092",
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
3. In `Program.cs` (non pushare la vera chiave X-Internal-Access):
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
5. Copia il secret ricevuto tramite https://eu.onetimesecret.com/ e inseriscilo nel comando seguente.
6. Nella cartella `svc-ai-vision-adapter`, esegui:
   ```bash
   echo '<secret>' \ | base64 -d > service-account.json
   ```