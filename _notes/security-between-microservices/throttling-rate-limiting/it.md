---
title: "Throttling e Rate Limiting"
categories: [microservices, it-security]
tags: [rate-limiting, api-security, scalability]
lang: it
locale: it
nav_order: 30
ref: note-throttling-rate-limiting
---
Il primo passo quando aumenta la domanda su un microservizio è la scalabilità, supportata dagli orchestratori di container tramite l’auto-scaling. Tuttavia, la scalabilità ha dei limiti: costi delle risorse, capacità esaurita dell’host o casi in cui scalare sarebbe dannoso, come durante gli attacchi denial-of-service. In queste situazioni, diventa fondamentale controllare il consumo.

- **Throttling** controlla l’uso da parte dei client limitando il traffico una volta raggiunte le soglie, evitando interruzioni totali.  
- **Quote** definiscono il numero di chiamate consentite su intervalli più lunghi, ad esempio al giorno o al mese, e sono spesso legate ad API monetizzate.  
- **Rate limiting** si concentra sulle chiamate simultanee, di solito misurate in transazioni al secondo.  

Le strategie di quota e throttling possono essere applicate a diversi livelli. Alcuni client, come le applicazioni interne, possono ricevere quote più elevate rispetto ai client di terze parti. Si può applicare anche granularità: limiti per utente (basati sull’identità dell’utente finale) o limiti per operazione (basati sul costo in risorse di ciascuna chiamata API).

Queste tecniche proteggono i microservizi dal sovraccarico e garantiscono un uso equo e sostenibile tra tutti i client.

<small> Fonte: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>