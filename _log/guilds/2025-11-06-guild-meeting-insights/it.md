---
title: "2025-11-06: Approfondimenti dal Guild Meeting"
categories: [microservices, it-security]
tags: [guild, individual, reflection]
lang: it
locale: it
nav_order: 16
ref: log-2025-11-06-guild-meeting-insights
---

##### Microservizi e Kubernetes

È stato sottolineato che l’esame riguarda ciò che *io* ho imparato sui microservizi e Kubernetes, sia in teoria che in pratica. Il progetto serve solo come prova del mio apprendimento — non è un obiettivo in sé. Devo quindi spiegare le mie scelte architetturali, le mie implementazioni e come ho acquisito la comprensione, ad esempio tramite video ed esperimenti.

Invece di elencare quali microservizi contiene il progetto, ha più senso spiegare come funzionano i microservizi in teoria e come li ho implementati io stessa in pratica. È stato chiarito che l’attenzione deve essere sulle mie scelte architetturali, sulle mie implementazioni e sulla mia comprensione dei principi — non una spiegazione dettagliata del codice o dei servizi degli altri. Inoltre, non è necessario che i servizi degli altri siano deployati in Kubernetes.

##### Sicurezza IT

**Il modello CIA come struttura**  
Thomas ha suggerito di organizzare la sicurezza informatica utilizzando la triade CIA: Confidentiality, Integrity e Availability. Il modello aiuta a valutare dove gli sforzi di sicurezza hanno più senso rispetto ai dati gestiti dal sistema. Un sistema completamente sicuro è impossibile, quindi la sicurezza consiste in equilibrio e priorità.

**Sicurezza Web e API**  
Abbiamo discusso del perché la sicurezza web e la sicurezza API siano strettamente collegate: entrambe si basano sul protocollo HTTP (tipicamente porta 80/443) e condividono molte superfici di attacco e contromisure. Internet è l’infrastruttura sottostante, mentre “web” è la parte del traffico che usa HTTP.

**Sicurezza del Database**  
La sicurezza del database si collega agli stessi tre pilastri:

- **Confidentiality:** Controllo degli accessi, privilegi minimi, crittografia dei dati a riposo e in transito  
- **Integrity:** Logging, audit trail e meccanismi che impediscono modifiche non rilevate  
- **Availability:** Configurazione del firewall, porte nascoste, protezione contro brute-force, backup e procedure di ripristino  

Abbiamo valutato quali elementi della CIA siano supportati da ciascun meccanismo:

- Controllo degli accessi e autenticazione → **Confidentiality**  
- Aggiornamenti di sicurezza regolari → principalmente **Confidentiality**, ma possono supportare tutti e tre  
- Sicurezza di rete e firewall → **Availability** e **Integrity**  
- Monitoraggio e logging → **Integrity**  

**Sicurezza Web**  
Abbiamo esaminato come gli stessi principi siano applicati anche alle applicazioni web. Minacce come XSS e SQL injection sfruttano HTTP come punto di ingresso.

Ancora una volta valutato tramite la triade CIA:

- Chi può accedere all’API? → **Confidentiality**  
- Possiamo fidarci dei dati che riceviamo? → **Integrity**  
- Come garantiamo la disponibilità? Serve un load balancer o un piano di ripristino? → **Availability**  

**Riepilogo**  
Quando si valuta la sicurezza di un sistema, si parte dal dominio e dalla documentazione, per poi considerare dove risiedono i rischi e quali misure siano sensate nella pratica. Non possiamo includere tutte le tecnologie, quindi è importante scegliere con attenzione.

##### Preparazione per i prossimi guild meeting  
**Microservizi e Kubernetes:** Discuteremo come presentare il nostro progetto e i punti tecnici all’esame.  
**Sicurezza IT:** Se desideriamo feedback sugli obiettivi di apprendimento, possiamo inviarli qualche giorno prima del prossimo incontro.