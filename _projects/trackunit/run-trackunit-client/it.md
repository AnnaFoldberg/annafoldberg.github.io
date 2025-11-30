---
title: "Trackunit: Eseguire Trackunit Client"
categories: [trackunit]
tags: [setup, final]
lang: it
locale: it
nav_order: 29
ref: run-trackunit-client
---

##### Run Trackunit Client
>Non supporta **TLS**.  

1. Apri trackunit-client in Android Studio.  
2. Nel terminale, vai a `~/Library/Android/sdk/platform-tools`.  
3. Esegui:
   ```bash
   ./adb reverse tcp:8080 tcp:8080
   ./adb reverse tcp:9000 tcp:9000
   ```
4. Avvia l’app tramite il pulsante `Run`.