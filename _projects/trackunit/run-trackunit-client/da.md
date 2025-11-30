---
title: "Trackunit: Kør Trackunit Client"
categories: [trackunit]
tags: [setup, final]
lang: da
locale: da
nav_order: 29
ref: run-trackunit-client
---

### Kør Trackunit Client
>Understøtter **ikke** TLS.  

1. Åbn trackunit-client i Android Studio.  
2. I terminalen, navigér til `~/Library/Android/sdk/platform-tools`.  
3. Kør:
   ```bash
   ./adb reverse tcp:8080 tcp:8080
   ./adb reverse tcp:9000 tcp:9000
   ```
4. Start appen via `Run`-knappen.