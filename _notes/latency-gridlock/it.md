---
title: "Latenza e Gridlock"
categories: [microservices]
tags: [architecture, reliability]
lang: it
locale: it
nav_order: 15
ref: note-latency-gridlock
---
Ogni chiamata a un servizio introduce una certa latenza. Per una singola chiamata questo è solitamente trascurabile, ma man mano che i percorsi delle richieste diventano più complessi, la latenza combinata può diventare evidente per il sistema.

Se non gestita, la latenza può accumularsi e portare a uno gridlock, in cui parti del sistema non sono in grado di procedere. Lo gridlock può anche verificarsi quando vengono introdotte dipendenze circolari, ad esempio quando un servizio chiama indirettamente se stesso attraverso altri servizi.

Per mitigare questo problema, è importante implementare una logica di timeout in tutto il sistema, in modo che la latenza o le chiamate circolari non si propaghino indefinitamente.

**Circuit breaker**

Un circuit breaker è un pattern comune utilizzato per gestire la latenza e lo gridlock. In questo pattern, quando si verificano timeout, il circuito si apre e viene eseguito un comportamento di fallback invece di chiamare ripetutamente il servizio interessato. Questo consente al sistema di continuare a funzionare con funzionalità limitata anziché fallire completamente. Quando i servizi interessati si riprendono, il circuito si chiude e il sistema riprende il funzionamento normale.

<small> Fonte: [LinkedIn Learning: Microservices Foundations](https://www.linkedin.com/learning/microservices-foundations-23469069?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>