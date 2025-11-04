---
title: "Trackunit: Comandi vs Eventi"
categories: [trackunit, microservices]
tags: [rabbitmq, architecture, naming-conventions]
lang: it
locale: it
nav_order: 7
ref: commands-events
---

##### Comandi
I comandi sono messaggi mirati. Esattamente un servizio deve gestire ogni messaggio. Si inviano al servizio che possiede la decisione o lo stato.  
*Direct exchange type* è il valore predefinito per i comandi. Garantisce che un solo consumer gestisca il comando. Utilizza un routing semplice con una routing key uguale al nome della coda.

##### Eventi
Gli eventi sono messaggi broadcast. Molti servizi possono iscriversi. Provengono da un servizio dopo che ha modificato il proprio stato.  
*Fanout exchange type* è il valore predefinito per gli eventi. Garantisce che ogni subscriber riceva tutti i messaggi ed è semplice ed efficiente.  
In alternativa, può essere utilizzato *topic exchange type* se sono necessarie sottoscrizioni filtrate. Ciò consente ai servizi di ascoltare solo sottoinsiemi di eventi in base ai pattern della routing key.

##### Convenzioni di denominazione nel progetto
- `ICommandPublisher` usa **SendAsync** per i comandi per enfatizzare la semantica diretta “esegui questo” e una maggiore aspettativa di consegna.  
- `IEventPublisher` usa **PublishAsync** per gli eventi per enfatizzare la semantica broadcast “è già avvenuto”.  
- Entrambi i nomi delle interfacce includono **publisher** perché pubblicano sempre su un broker (RabbitMQ).