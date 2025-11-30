---
title: "Trackunit: Run Trackunit Client"
categories: [trackunit]
tags: [setup, final]
lang: en
locale: en
nav_order: 29
ref: run-trackunit-client
---

## Run Trackunit Client
>Does **not** support TLS.  

1. Open trackunit-client in Android Studio.  
2. In the terminal, navigate to `~/Library/Android/sdk/platform-tools`.  
3. Run:
   ```bash
   ./adb reverse tcp:8080 tcp:8080
   ./adb reverse tcp:9000 tcp:9000
   ```
4. Start the app using the `Run` button.