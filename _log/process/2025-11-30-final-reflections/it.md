---
title: "2025-11-30: Riflessioni Finali"
categories: [microservices, kubernetes, it-security]
tags: [individual, reflection, process, project]
lang: it
locale: it
nav_order: 20
ref: log-2025-11-30-final-reflections
---

##### Obiettivi di Apprendimento Iniziali

**Microservizi e Kubernetes**  
All’inizio del semestre mi ero posta l’obiettivo di acquisire una comprensione approfondita di come microservizi e Kubernetes possano essere utilizzati nella pratica per costruire sistemi scalabili e flessibili. Volevo saper progettare un’architettura in cui i componenti fossero ben definiti, debolmente accoppiati e sviluppabili e gestibili in modo indipendente. Kubernetes avrebbe dovuto garantire stabilità, scalabilità e affidabilità operativa.

**Sicurezza IT**  
Desideravo integrare la sicurezza come parte naturale dell’architettura. Il mio obiettivo era identificare i principali rischi di sicurezza in un ambiente a microservizi basato su Kubernetes e scegliere meccanismi adeguati per proteggere dati, comunicazione e infrastruttura.

##### Processo

Ho iniziato il semestre raccogliendo e organizzando informazioni, poiché non avevo esperienza precedente con queste tecnologie. Ho seguito percorsi formativi su LinkedIn Learning riguardanti microservizi, Kubernetes e sicurezza IT correlata, ottenendo così una visione generale dei concetti essenziali dal punto di vista del design, dell’uso e della sicurezza. Allo stesso tempo, avevo bisogno di comprendere rapidamente microservizi e message broker per permettere al team di individuare presto un’architettura sensata.

Poiché anche altri membri del team desideravano approfondire concetti legati ai microservizi e alla messaggistica, abbiamo suddiviso le responsabilità all’interno del sistema. Io ho assunto il ruolo di gestire l’API gateway, il server GraphQL e l’orchestrazione centrale dell’analisi. Ciò ha permesso a chi era più orientato al backend di lavorare sui servizi con maggiore logica applicativa. La suddivisione rifletteva anche i due flussi principali del sistema: caricamento dell’immagine e analisi dell’immagine.

Dopo aver approfondito i principi dei microservizi, RabbitMQ e GraphQL, ho sviluppato un progetto di spike, **TeaApp**, dove ho sperimentato architettura a microservizi, autenticazione tramite Microsoft Entra ID, Kong come API gateway, operazioni GraphQL e pubblicazione di eventi tramite RabbitMQ. Questo mi ha fornito esperienza pratica e una solida base per il progetto Trackunit.

Ho poi proseguito con il percorso Kubernetes su LinkedIn Learning, passando rapidamente alla documentazione ufficiale su Kubernetes, che ho trovato facilmente applicabile alla pratica. Parallelamente ho seguito corsi Udemy per comprendere relazioni più complesse, soprattutto riguardanti la sicurezza e le interazioni tra i componenti Kubernetes.

Avevo anche pianificato di approfondire OWASP API Security Top 10, OWASP Kubernetes Top 10 e la Cyber Security Roadmap. Anche se li ho consultati occasionalmente, ho scelto di non esplorarli a fondo, preferendo concentrarmi su un numero più ridotto di meccanismi di sicurezza essenziali e ricorrenti nei vari corsi e articoli.

##### Prototipo Trackunit in Docker

Ho iniziato l’implementazione del progetto Trackunit con un prototipo che includeva solo i servizi di cui ero responsabile: l’API gateway (**kong** e **oauth2-proxy**), **graph-gateway** e **svc-analysis-orchestrator**.

**InfraCore** raggruppava tutti i servizi e i componenti infrastrutturali in un unico ambiente docker-compose, consentendo l’esecuzione congiunta tramite una rete Docker condivisa. Ho anche sviluppato un **ClientConsole** per testare l’autenticazione, il flusso GraphQL e verificare la corretta pubblicazione degli eventi.

In questa fase il team voleva spostare la comunicazione da RabbitMQ a Kafka. Ho sperimentato questa transizione ma ho scelto di mantenere RabbitMQ come mio message broker, grazie alla migliore comprensione di base che avevo costruito e alla priorità assegnata ad altre aree. Poiché il team ha proseguito con Kafka, ho pianificato di introdurre successivamente un **messaging bridge** capace di convertire eventi Kafka in eventi RabbitMQ. In origine doveva essere bidirezionale, ma il team ha deciso che **svc-ai-vision-adapter** dovesse ricevere `tu.image.uploaded` direttamente da **tu-ingestion-service**, bypassando **svc-analysis-orchestrator**. Questo ha ridotto il ruolo architetturale dell’orchestratore a un semplice notifier, anche se ho scelto di mantenerlo perché sarebbe fondamentale in una versione più scalabile del sistema.

##### Transizione a Kubernetes

Successivamente ho migrato il sistema da docker-compose a Kubernetes. Ho scelto di eseguire tutto localmente in `kind`, poiché il mio obiettivo era apprendere il funzionamento di Kubernetes e i meccanismi di sicurezza, piuttosto che una soluzione basata sul cloud. Ho selezionato risorse Kubernetes specifiche che affrontano aspetti sia operativi sia di sicurezza: deployments, services, secrets, namespaces e network policies.

Ho installato i componenti infrastrutturali tramite Helm chart con values file per garantire configurazioni stabili, coerenti e orientate alla sicurezza. Questo mi ha fornito un servizio LoadBalancer per **kong-proxy**, accessibile tramite `cloud-provider-kind`.

Per la gestione e il monitoraggio del cluster ho utilizzato `k9s`, che offre una panoramica efficiente di componenti e log.

Successivamente ho lavorato sulla gestione di chiavi e certificati per stabilire una corretta terminazione TLS tra il client e **kong-proxy**. In questo contesto, ho pianificato di estendere la configurazione affinché **kong** fungesse non solo da API Gateway ma anche da Ingress Controller, con una risorsa Ingress che definisse routing e configurazione TLS per il traffico esterno in ingresso al cluster. Questo garantisce che tutto il traffico al di fuori del cluster sia crittografato in transito e che i certificati vengano terminati in modo sicuro su **kong-proxy**.

Ho scelto di non introdurre mTLS — né tra il client e **kong-proxy**, né internamente tra i pod tramite un service mesh. Una soluzione del genere sarebbe eccessiva rispetto alla portata del progetto e agli obiettivi formativi, ma potrebbe essere introdotta in una futura iterazione orientata a un modello di sicurezza più completo.  

Avevo inizialmente previsto di sperimentare Kubernetes con **TeaApp** come fase intermedia, ma ho ritenuto che non fosse necessario, perché la configurazione docker-compose esistente nel progetto Trackunit poteva essere trasferita ai manifest Kubernetes in modo relativamente semplice.  

##### Integrazione con Servizi Esterni

Prima di integrare i microservizi del team, ho introdotto la versioning delle immagini dei container. Ciò mi ha permesso di eseguire rollback dei deployment quando nuove versioni causavano problemi inattesi nella comunicazione tra i servizi.

Nel momento di integrare i microservizi del team con il mio cluster, ho scelto di non deployare tali servizi in Kubernetes. La configurazione di componenti aggiuntivi come **Redpanda**, **MinIO** e una **sidecar Tailscale**, oltre ai microservizi del team, avrebbe introdotto una complessità significativa senza un corrispondente valore formativo in questa fase. Poiché il progetto Trackunit è un proof-of-concept, ho dato priorità al funzionamento e alla sicurezza di Kubernetes. In una futura iterazione, tali servizi potranno essere migrati nel cluster.

Come parte dell’integrazione, ho implementato **svc-messaging-bridge**, che ha fornito la connessione tra Kafka e RabbitMQ.

##### Conclusione

Durante questo processo ho raggiunto gli obiettivi di apprendimento che avevo definito all’inizio del semestre. Ciò ha portato a risultati di apprendimento individuali concreti, collegati alla tassonomia di Bloom, che offrono una panoramica chiara sia del livello teorico sia di quello pratico su cui ho lavorato.