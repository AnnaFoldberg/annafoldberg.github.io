---
title: "Trackunit: Configurare Tailscale & Redpanda"
categories: [trackunit]
tags: [configuration, infrastructure, kafka, final]
lang: it
locale: it
nav_order: 25
ref: configure-tailscale-redpanda
---

##### Configure Tailscale & Redpanda
1. Installa:
   ```bash
   curl -fsSL https://tailscale.com/install.sh | sh
   ```
2. Avvia:
   ```bash
   sudo tailscale up
   ```
3. Trova il tuo IP personale di Tailscale. Questo viene utilizzato al posto di `100.106.102.107` in tutte le configurazioni seguenti.
4. Configura Redpanda:
   ```bash
   docker run -d --name=redpanda -p 9092:9092 -p 9644:9644 -v redpanda-data:/var/lib/redpanda/data docker.redpanda.com/redpandadata/redpanda:latest redpanda start --overprovisioned --smp 1 --memory 1G --kafka-addr internal://0.0.0.0:9092 --advertise-kafka-addr internal://100.106.102.107:9092
   ```
5. Entra nel container Redpanda:
   ```bash
   docker exec -it redpanda /bin/bash
   ```
6. All’interno del container Redpanda, crea i topic Kafka:
   ```bash
   rpk topic create tu.images.uploaded --partitions 4 --replicas 1  
   rpk topic create tu.recognition.completed --partitions 4 --replicas 1  
   ```
7. Esci dal container Redpanda.
8. Crea un Tailscale funnel:
   ```bash
   tailscale funnel 5104
   ```
   Copia `https://<address>.ts.net` per utilizzi successivi.