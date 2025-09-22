---
title: "2025-09-18: Approfondimenti dal Guild Meeting"
categories: [microservices, kubernetes]
tags: [guild, individual, reflection]
lang: it
locale: it
nav_order: 9
ref: log-2025-09-18-guild-meeting-insights
---
##### Microservizi e Kubernetes  
**Server GraphQL e struttura dei servizi**  
Abbiamo discusso che in un sistema più piccolo probabilmente ha più senso utilizzare un singolo server GraphQL, anche se ciò comporta un single point of failure. Aggiungere più server introdurrebbe soltanto complessità non necessaria e diventa rilevante solo in soluzioni di dimensioni maggiori.  
Abbiamo inoltre concluso che ha comunque senso mantenere il servizio di analisi, anche se il server GraphQL già si occupa di instradare le chiamate dei client verso i microservizi corretti. La differenza è che, mentre il server GraphQL gestisce solo il routing dal client, il servizio di analisi può contenere logica di business (ad esempio, come il sistema deve reagire se manca un’immagine), che non dovrebbe risiedere nel server GraphQL.  

**Autenticazione con proxy**  
Ho chiarito che un proxy OAuth può essere una soluzione valida quando Kong OSS non supporta direttamente OIDC.  

**Documentazione e collaborazione**  
Abbiamo anche parlato dell’importanza della documentazione in un’architettura a microservizi. Anche se il team non ha bisogno di conoscere tutti i dettagli tecnici, è importante che sappia quali chiamate e parametri utilizzare. Dobbiamo lavorare continuamente per rendere la documentazione chiara e accessibile, mantenendo al contempo una visione d’insieme mentre il sistema cresce.  

**Messaging e sincronizzazione**  
Abbiamo inoltre discusso dei message broker e di come garantiscano la comunicazione asincrona, in modo che mittente e destinatario non debbano essere online nello stesso momento, e di come, in una certa misura, possano preservare l’ordine dei messaggi, ad esempio all’interno di una coda o di una partizione, anche quando i servizi sono temporaneamente inattivi.  