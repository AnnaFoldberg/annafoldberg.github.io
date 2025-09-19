---
title: "Fondamenti di Sicurezza"
categories: [microservices, it-security]
tags: [cia-triad, authentication, authorization, attack-surface]
lang: it
locale: it
nav_order: 16
ref: note-security-fundamentals
---
**Confidenzialità:** Controllo dell’accesso ai dati per prevenire divulgazioni non autorizzate  

**Integrità:** Garantire che i dati non vengano manomessi e siano affidabili  

**Disponibilità:** Gli utenti autorizzati possono accedere ai dati quando necessario  

Per raggiungere questi obiettivi, strategie e controlli di sicurezza vengono scelti e applicati all’architettura di un sistema nella misura in cui sono richiesti.

![CIA triad](../../assets/images/notes/microservices/security-fundamentals.png)

**Controllo degli accessi**

1. Autenticazione: Stabilire l’identità  
2. Autorizzazione: Verificare i privilegi di accesso  
3. Accesso alle risorse  

**Autenticazione**

- Verifica dell’identità delle parti che accedono a un sistema  
- Umane o non umane (sistemi)  
- Credenziali o token provano l’identità  
- I sistemi altamente sensibili possono utilizzare l’autenticazione a più fattori (MFA)  

**Autorizzazione**

- Garantisce l’accesso autorizzato e l’uso delle risorse  
- Identificazione e applicazione dei privilegi  
- Può essere delegata a terze parti  

**Fiducia**

- Determina in quale misura qualcosa è ritenuto vero.  

**Superficie di attacco**

Comprende tutti i percorsi che possono essere utilizzati per far entrare o uscire dati da un’applicazione. L’interfaccia utente di un sistema, le porte aperte, le API e il database possono tutti rappresentare opportunità per un attaccante di compromettere un sistema. Pertanto sono considerati parte della superficie di attacco. Ridurre e rafforzare la superficie di attacco è una strategia efficace per migliorare la sicurezza di un sistema.

<small> Fonte: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>