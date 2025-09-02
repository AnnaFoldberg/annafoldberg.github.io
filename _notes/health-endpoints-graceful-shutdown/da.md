---
title: "Health Endpoints og Graceful Shutdown"
categories: [microservices, kubernetes]
tags: [health-checks, graceful-shutdown, lifecycle-management, best-practices]
lang: da
locale: da
nav_order: 3
ref: note-health-endpoints-graceful-shutdown
---
##### Health Endpoints
- **Liveness:** Er jeg i live? Hvis false genstarter platformen containeren  
- **Readiness:** Kan jeg håndtere trafik? Hvis false sættes trafikken på pause (f.eks. under opstart eller når afhængigheder er nede)  
- **Startup** (valgfri): Har jeg afsluttet opstarten?  

Eksempel:  
- /healthz (liveness): returnerer 200 hvis main loopet kører  
- /readyz (readiness): returnerer 200 kun hvis DB/cache/message-bus tjekket består  

```javascript
app.get('/healthz', (req, res) => res.sendStatus(200)); // liveness
app.get('/readyz', async (req, res) => {
  const ok = await checkDbAndCache();
  res.sendStatus(ok ? 200 : 503);
});
```

##### Health Endpoints
Når platformen vil stoppe containeren, sender den SIGTERM. Din app bør:
1.	Stoppe med at acceptere nye forespørgsler
2.	Afslutte igangværende arbejde
3.	Lukke forbindelser og derefter afslutte rent, inden SIGKILL  
Dette undgår halvfærdige forespørgsler eller korrupte jobs.  

Eksempel:
```javascript
const server = app.listen(process.env.PORT || 3000);

process.on('SIGTERM', () => {
  server.close(() => process.exit(0)); // stop accepting, finish in-flight
  setTimeout(() => process.exit(1), 10000); // hard timeout fallback
});
```