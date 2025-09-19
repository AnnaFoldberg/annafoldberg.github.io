---
title: "Operazioni Idempotenti"
categories: [microservices, kubernetes]
tags: [reliability]
lang: it
locale: it
nav_order: 4
ref: note-idempotent-operations
---
Un’operazione idempotente è un’operazione che ha lo stesso effetto indipendentemente da quante volte viene eseguita ⇒ lo stato finale sarà sempre lo stesso.  

##### Idempotenti per progettazione
- **GET:** Restituisce sempre le stesse informazioni.  
- **DELETE:** Una volta eliminato, ripetere la chiamata non lo elimina ulteriormente.  
- **PUT:** Impostare ripetutamente lo stato della risorsa lo mantiene invariato.  

##### Non idempotenti per progettazione
- **POST:** Per impostazione predefinita, POST crea una nuova risorsa o attiva un’azione.  

##### Come rendere idempotenti operazioni non idempotenti
- **Chiave di idempotenza:** Se i retry devono essere sicuri (ad es. pagamenti o invio ordini), puoi rendere una POST idempotente richiedendo una chiave di idempotenza univoca.  
    - Il client invia una chiave univoca con la richiesta.  
    - Il server garantisce che la stessa chiave = stesso risultato.  
- **Upsert invece di insert:** Usa PUT al posto di POST. PUT imposta l’intero stato, quindi se ripetuto porta comunque allo stesso stato finale.  
- **Verifiche di stato:** Prima di scrivere, controlla se è già stato fatto.  

##### Cosa dovrebbe essere idempotente nei microservices
- **Operazioni mutanti che possono essere ripetute:** Se la rete fallisce, i client/Kubernetes possono riprovare automaticamente. Con le code di messaggi, i broker possono anche ritrasmettere. Se l’operazione non è idempotente, i retry possono causare doppie prenotazioni, doppi addebiti o lavori duplicati.  
- **Compiti infrastrutturali:** Riapplicare la stessa configurazione (ad es. un manifest Kubernetes) dovrebbe lasciare il sistema nello stesso stato senza causare errori.  

##### Cosa non deve essere idempotente
- **Operazioni POST quando i duplicati sono azioni aziendali intenzionali:** Ad es. scrittura in un audit log, creazione di messaggi in chat, mettere “mi piace” a un post, ecc. In questi casi, chiamare due volte dovrebbe creare due voci.  