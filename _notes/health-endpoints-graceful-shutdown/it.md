---
title: "Health Endpoints e Graceful Shutdown"
categories: [microservices, kubernetes]
tags: [health-checks, graceful-shutdown, lifecycle-management, best-practices]
lang: it
locale: it
nav_order: 3
ref: note-health-endpoints-graceful-shutdown
---
##### Health Endpoints
- **Liveness:** Sono vivo? Se falso, la piattaforma riavvia il container  
- **Readiness:** Posso servire traffico? Se falso, il traffico viene sospeso (ad es. durante l’avvio o quando le dipendenze non sono disponibili)  
- **Startup** (opzionale): Ho terminato l’avvio?  

Esempio:  
- /healthz (liveness): restituisce 200 se il loop principale è in esecuzione  
- /readyz (readiness): restituisce 200 solo se i controlli su DB/cache/message-bus vanno a buon fine  

```javascript
app.get('/healthz', (req, res) => res.sendStatus(200)); // liveness
app.get('/readyz', async (req, res) => {
  const ok = await checkDbAndCache();
  res.sendStatus(ok ? 200 : 503);
});
```

##### Graceful Shutdown
Quando la piattaforma vuole fermare il tuo container, invia SIGTERM. La tua app dovrebbe:
1.	Smettere di accettare nuove richieste
2.	Terminare il lavoro in corso
3.	Chiudere le connessioni ed uscire in modo pulito prima di SIGKILL
Questo evita richieste parzialmente elaborate o lavori corrotti.

Esempio:
```javascript
const server = app.listen(process.env.PORT || 3000);

process.on('SIGTERM', () => {
  server.close(() => process.exit(0)); // stop accepting, finish in-flight
  setTimeout(() => process.exit(1), 10000); // hard timeout fallback
});
```