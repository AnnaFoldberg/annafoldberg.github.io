---
title: "Sicurezza delle Container Image"
categories: [microservices, it-security]
tags: [containers, image-security]
lang: it
locale: it
nav_order: 32
ref: note-container-image-security
---
Le container image vengono create a partire dai Dockerfile, che definiscono le istruzioni per aggiungere e configurare un microservizio. Ogni istruzione aggiunge un nuovo livello all’immagine, partendo da una base image, spesso fornita da fonti affidabili come Microsoft, Red Hat o distribuzioni Linux ufficiali. Una volta create, le immagini vengono archiviate in un registro e scaricate per essere eseguite come container.

La sicurezza inizia utilizzando immagini di base ufficiali provenienti da repository affidabili. Immagini non verificate possono introdurre codice dannoso, creando un attacco alla supply chain che compromette i sistemi di produzione. Le immagini ufficiali vengono aggiornate regolarmente per correggere vulnerabilità, quindi rimanere aggiornati è fondamentale.

**Pratiche di manutenzione delle immagini:**

- Scansionare regolarmente i registri per vulnerabilità con strumenti automatici come Snyk  
- Ricostruire le immagini con base image aggiornate quando vengono scoperte CVE  
- Rilanciare i container per sostituire le istanze obsolete  
- Preferire base image ridotte per ridurre la superficie di attacco e limitare le opzioni dell’attaccante  

Queste pratiche assicurano che i microservizi containerizzati rimangano sicuri e resilienti contro minacce in continua evoluzione.

<small> Fonte: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>