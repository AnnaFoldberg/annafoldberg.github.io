---
title: "Trackunit: Configure Tailscale & Redpanda"
categories: [trackunit]
tags: [configuration, infrastructure, kafka, final]
lang: da
locale: da
nav_order: 25
ref: configure-tailscale-redpanda
---

##### Configure Tailscale & Redpanda
1. Installér:
   ```bash
   curl -fsSL https://tailscale.com/install.sh | sh
   ```
2. Kør:
   ```bash
   sudo tailscale up
   ```
3. Find din personlige Tailscale-IP. Denne bruges i stedet for `100.106.102.107` i alle konfigurationerne nedenfor.
4. Konfigurer Redpanda:
   ```bash
   docker run -d --name=redpanda -p 9092:9092 -p 9644:9644 -v redpanda-data:/var/lib/redpanda/data docker.redpanda.com/redpandadata/redpanda:latest redpanda start --overprovisioned --smp 1 --memory 1G --kafka-addr internal://0.0.0.0:9092 --advertise-kafka-addr internal://100.106.102.107:9092
   ```
5. Gå ind i Redpanda-containeren:
   ```bash
   docker exec -it redpanda /bin/bash
   ```
6. Inde i Redpanda-containeren, opret Kafka-topics:
   ```bash
   rpk topic create tu.images.uploaded --partitions 4 --replicas 1  
   rpk topic create tu.recognition.completed --partitions 4 --replicas 1  
   ```
7. Forlad Redpanda-containeren.
8. Opret tailscale funnel:
   ```bash
   tailscale funnel 5104
   ```
   Kopiér `https://<address>.ts.net` til senere brug.