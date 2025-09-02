---
title: "Health Endpoints and Graceful Shutdown"
categories: [microservices, kubernetes]
tags: [health-checks, graceful-shutdown, lifecycle-management, best-practices]
lang: en
locale: en
nav_order: 3
ref: note-health-endpoints-graceful-shutdown
---
##### Health Endpoints
- **Liveness:** Am I alive? If false, the platform restarts the container  
- **Readiness:** Can I serve traffic? If false, traffic is paused (e.g., during startup or while dependencies are down)  
- **Startup** (optional): Have I finished booting?  

Example:
- /healthz (liveliness): returns 200 if main loop runs  
- /readyz (readiness): returns 200 only if DB/cache/message-bus checks pass  

```javascript
app.get('/healthz', (req, res) => res.sendStatus(200)); // liveness
app.get('/readyz', async (req, res) => {
  const ok = await checkDbAndCache();
  res.sendStatus(ok ? 200 : 503);
});
```

##### Graceful Shutdown
When the platofrm wants to stop your container, it sends SIGTERM. Your app should:
1. Stop accepting new requests
2. Finish in-flight work
3. Close connections, then exit cleanly before SIGKILL
This avoids half-processed requests or corrupted jobs.

Example:
```javascript
const server = app.listen(process.env.PORT || 3000);

process.on('SIGTERM', () => {
  server.close(() => process.exit(0)); // stop accepting, finish in-flight
  setTimeout(() => process.exit(1), 10000); // hard timeout fallback
});
```