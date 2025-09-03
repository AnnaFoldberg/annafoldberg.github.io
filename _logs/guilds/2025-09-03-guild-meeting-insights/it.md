---
title: "2025-09-03: Approfondimenti dal guild meeting"
categories: [microservices, kubernetes, it-security]
tags: [guild, individual, reflection]
lang: it
locale: it
nav_order: 7
ref: log-2025-09-03-guild-meeting-insights
---
**Microservizi e Kubernetes**  
Durante l’incontro abbiamo discusso, tra le altre cose, della scelta dei database e abbiamo considerato se PostgreSQL potesse essere una buona opzione nel nostro contesto. PostgreSQL è un database relazionale a oggetti, che può gestire sia dati relazionali classici sia offrire la possibilità di memorizzare oggetti direttamente. Questo solleva la domanda se vogliamo sfruttare questa flessibilità o se invece rischierebbe di introdurre complessità inutile.  
In particolare, in relazione ai microservizi, è stato sottolineato che ogni microservizio ha tipicamente il proprio database. Se PostgreSQL venisse utilizzato per memorizzare oggetti direttamente, potremmo finire con lo stesso file o oggetto duplicato in più database, creando ridondanza e aumentando l’uso dello storage. In un’architettura più classica con un unico database comune combinato con object storage, si ripeterebbe solo il link all’oggetto, non i dati stessi. La riflessione riguarda quindi se PostgreSQL debba essere usato come soluzione unica per dati relazionali e simili a oggetti nei microservizi, o se sia in pratica più opportuno continuare con database relazionali in combinazione con object storage dedicato.  

**Sicurezza IT**  
Siamo stati ricordati dell’importanza di esaminare quali regolamenti si applicano nell’ambito del nostro progetto e del nostro ambito di sicurezza e come possano influenzare il nostro lavoro e le nostre priorità. Non si tratta solo dell’implementazione tecnica, ma anche di garantire che operiamo costantemente nel rispetto delle normative vigenti.