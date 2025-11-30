---
title: "Trackunit: Configure Tailscale & Redpanda"
categories: [trackunit]
tags: [configuration, infrastructure, kafka, final]
lang: en
locale: en
nav_order: 25
ref: configure-tailscale-redpanda
---

##### Configure Tailscale & Redpanda
1. Install:
   ```bash
   curl -fsSL https://tailscale.com/install.sh | sh
   ```
2. Run:
   ```bash
   sudo tailscale up
   ```
3. Find your personal Tailscale IP. This is used in place of `100.106.102.107` in all configurations below.
4. Configure Redpanda:
   ```bash
   docker run -d --name=redpanda -p 9092:9092 -p 9644:9644 -v redpanda-data:/var/lib/redpanda/data docker.redpanda.com/redpandadata/redpanda:latest redpanda start --overprovisioned --smp 1 --memory 1G --kafka-addr internal://0.0.0.0:9092 --advertise-kafka-addr internal://100.106.102.107:9092
   ```
5. Enter the Redpanda container:
   ```bash
   docker exec -it redpanda /bin/bash
   ```
6. Inside the Redpanda container, create Kafka topics:
   ```bash
   rpk topic create tu.images.uploaded --partitions 4 --replicas 1  
   rpk topic create tu.recognition.completed --partitions 4 --replicas 1  
   ```
7. Exit the Redpanda container.
8. Create a Tailscale funnel:
   ```bash
   tailscale funnel 5104
   ```
   Copy `https://<address>.ts.net` for later use.