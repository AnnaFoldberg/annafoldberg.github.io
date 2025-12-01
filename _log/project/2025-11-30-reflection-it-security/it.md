---
title: "2025-11-30: Il Ruolo della Sicurezza Informatica nel Progetto"
categories: [it-security, kubernetes, microservices, trackunit]
tags: [reflection, project, individual]
lang: it
locale: it
nav_order: 19
ref: log-2025-11-30-reflection-it-security
---

##### Misure di Sicurezza Coperte dal Progetto

Il progetto ha permesso di lavorare con diversi meccanismi di sicurezza fondamentali in un ambiente cloud-native. L’aspetto più importante è stato il controllo degli accessi, dove ho implementato rate-limiting in Kong e l’autenticazione basata su OIDC tramite oauth2-proxy, seguita dalla validazione dei token in graph-gateway. Questo ha garantito che solo i client con JWT validi potessero chiamare l’API gateway e che le richieste non autorizzate o di abuso venissero bloccate subito all’inizio del flusso.

Ho anche lavorato con la sicurezza del livello di trasporto configurando la terminazione TLS tramite un Ingress Controller con certificati self-signed, così da criptare il traffico tra il client e il punto di ingresso del sistema.

Inoltre, ho configurato Network Policies per limitare il traffico tra i pod e ottenere un controllo più granulare su quali servizi potessero comunicare. Questo ha ridotto la superficie di attacco all’interno del cluster permettendo solo il traffico necessario e bloccando tutto il resto per impostazione predefinita.

Infine, ho utilizzato Kubernetes Secrets per gestire informazioni sensibili in modo più sicuro, evitando di inserirle direttamente nei deployments o nei values file. Questo ha fornito una separazione più chiara tra configurazione e segreti e un approccio più responsabile alla gestione delle credenziali.

##### Misure di Sicurezza Non Coperte dal Progetto

Ci sono anche aspetti della sicurezza che il progetto non include volutamente. Ad esempio, non ho utilizzato un service mesh per mTLS tra i servizi. Anche se garantirebbe una gestione dell’identità più forte e una cifratura interna al cluster, avrebbe aggiunto una complessità significativa non in linea con la portata e gli obiettivi formativi del progetto.

Non ho lavorato nemmeno con logging e tracing centralizzati. In un ambiente di produzione sono importanti per rilevare attività sospette e investigare problemi, ma qui ho scelto di concentrarmi sui meccanismi fondamentali come controllo degli accessi, TLS, Secrets e restrizioni di rete.

##### Riepilogo

Il progetto copre gli elementi di sicurezza più rilevanti per un sistema di queste dimensioni, tra cui cifratura client-cluster, autenticazione, rate-limiting, validazione dei token, gestione dei Secrets e limitazione del traffico di rete. Tecnologie più avanzate come i service mesh e la piena observability sono state volutamente escluse per mantenere il focus sugli aspetti con il maggiore valore formativo senza introdurre complessità non necessaria.