---
title: "2025-10-23: Approfondimenti dal Guild Meeting"
categories: [microservices, it-security]
tags: [guild, individual, reflection]
lang: it
locale: it
nav_order: 13
ref: log-2025-10-23-guild-meeting-insights
---
##### Microservizi e Kubernetes  
**Logging e Sicurezza nei Microservizi**  
Abbiamo discusso di quanto il logging sia davvero essenziale e di quanto dovrebbe essere presente nel progetto. Ho deciso di mantenerlo a un livello teorico: è importante capire come logging e tracing vengano utilizzati per ottenere una visione d’insieme e individuare errori in un sistema distribuito, ma non ritengo necessario implementarlo completamente in questa fase. Mi concentro invece su aspetti di sicurezza più concreti come JWT, OAuth e i service mesh, poiché svolgono un ruolo più centrale nell’architettura complessiva della sicurezza del sistema.  

**Messaging**  
Per quanto riguarda la messaggistica, abbiamo concordato sull’importanza di approfondire un po’ questo tema, poiché i microservizi raramente hanno senso senza un meccanismo di comunicazione. Utilizzo RabbitMQ per la mia parte del sistema, mentre altri membri del gruppo possono scegliere tecnologie diverse. L’aspetto più importante è saper motivare la propria scelta.  

**Prospettiva**  
Abbiamo inoltre riflettuto se un’architettura a microservizi sia effettivamente adatta alle dimensioni del progetto. Molti degli elementi più complessi diventano veramente utili solo su larga scala, quando ci sono numerosi servizi indipendenti.  

**Spikes**  
Abbiamo parlato di utilizzare piccoli spike per sperimentare nuove tecnologie in prove limitate. Questo approccio permette di acquisire esperienza pratica e testare le idee su scala ridotta prima di implementarle nel progetto completo.  

##### Sicurezza IT  
**Ambito**  
È stato chiarito che l’attenzione per la sicurezza informatica rimane entro i limiti della nostra definizione, e che non ci si aspetta che trattiamo argomenti oltre gli obiettivi di apprendimento e i contenuti presenti nei nostri portfolio.  

**HashiCorp Vault**  
Abbiamo discusso che HashiCorp Vault è facile da configurare con Kubernetes, ma che in produzione dovrebbe essere eseguito su un server separato o in una macchina virtuale, piuttosto che in un normale container, poiché richiede accesso diretto al sistema e uno storage stabile.  
Per scopi dimostrativi o prototipali, tuttavia, può essere eseguito senza problemi in un container.  
HashiCorp supporta l’assegnazione di ruoli e token che scadono automaticamente o vengono disattivati quando i relativi ruoli vengono chiusi. Emmette token solo agli utenti che necessitano di accesso e, tramite l’audit log, è possibile vedere chi ha avuto accesso a quali risorse.  

**Corso Kubernetes consigliato**  
[Certified Kubernetes Administrator (CKA) – LinkedIn Learning](https://www.linkedin.com/learning/certified-kubernetes-administrator-cka-cert-prep-25818035/lesson-11-lab-solution-setting-up-storage?u=57075649)