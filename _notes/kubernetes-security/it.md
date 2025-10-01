---
title: "Kubernetes Security"
categories: [it-security, kubernetes]
tags: [security-context, readonly-filesystem, snyk, updates]
lang: it
locale: it
nav_order: 54
ref: note-kubernetes-security
---
Come qualsiasi sistema esposto a Internet, i clusters Kubernetes sono bersagli per gli attaccanti. Gli obiettivi comuni includono il furto di dati, il dirottamento delle risorse del cluster per il mining di criptovalute o l’avvio di attacchi distributed denial-of-service (DDoS). Sebbene le minacce si evolvano, ci sono diverse best practices che possono essere applicate immediatamente per migliorare la sicurezza del cluster.

##### Security Context
I container dovrebbero essere configurati con impostazioni di sicurezza restrittive per ridurre il rischio in caso di accesso da parte di un attaccante. Le pratiche chiave includono:  
- Eseguire i container come utenti non-root.  
- Disabilitare il privilege escalation.  
- Rimuovere tutte le Linux capabilities non esplicitamente necessarie.  

##### Read-Only Root Filesystem
Impostare il filesystem del container come read-only con `readOnlyRootFilesystem: true`, per impedire agli attaccanti di modificare file o installare software all’interno del container.

##### Scansione con Strumenti di Sicurezza
Utilizzare strumenti come **Snyk** per scansionare i manifest Kubernetes alla ricerca di misconfigurations. Le scansioni evidenziano problemi come filesystem root scrivibili, controlli utente mancanti e assenza di restrizioni sui privilegi. L’esecuzione regolare delle scansioni aiuta a garantire configurazioni sicure.

##### Aggiornare Kubernetes Regolarmente
Mantenere i clusters Kubernetes aggiornati, soprattutto quando vengono rilasciate patch di sicurezza. Monitorare il [database CVE](https://cve.mitre.org/) per le vulnerabilità note di Kubernetes e applicare tempestivamente le correzioni.

##### Riepilogo
Migliorare la sicurezza di Kubernetes inizia con misure semplici ma efficaci: configurare i Pods con security contexts, imporre filesystem in sola lettura, scansionare i manifest con strumenti di sicurezza e aggiornare regolarmente Kubernetes per applicare le patch di sicurezza.

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>