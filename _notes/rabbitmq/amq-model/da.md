---
title: "RabbitMQ: AMQ Model"
categories: [microservices]
tags: [rabbitmq, communication]
lang: da
locale: da
nav_order: 7
ref: note-amq-model
---
Dette er modellen for, hvordan RabbitMQ er opbygget:  
![Advanced Messaging Queuing Protocol](../../../assets/images/notes/rabbitmq/amq-model/amqp.png)

Alt starter med en applikation, der vil sende en besked. Denne kaldes publisheren, eller nogle gange produceren.  

Publisheren forbinder til en message broker og publicerer beskeden til en exchange. Publisheren kan også sende en routing key sammen med beskeden.  

Exchangen videresender beskeden til køerne. Exchangen bruger visse regler til at bestemme, hvilken kø beskeden skal rutes til. Disse regler kaldes bindings, og de kan bruge routing key’en, som publisheren inkluderede.  

Til sidst sendes beskeden til den modtagende applikation, kaldet consumeren. Brokeren vil skubbe beskeden til en abonnent, men det er også muligt for consumere at hente beskederne selv. Flere applikationer kan abonnere på den samme kø, men kun én af dem vil modtage en enkelt besked.  

Som sidste trin sender consumeren en bekræftelse (acknowledgment) tilbage til message brokeren. Dette signalerer til brokeren, at beskeden kan slettes fra køen.  

##### Konfiguration af køer og exchanges
- **Durability**
    - Durability er ikke det samme som persistens  
    - En durable kø eller exchange overlever en RabbitMQ-genstart, mens transiente ikke gør.  
        - Det betyder ikke, at ikke-leverede beskeder automatisk gemmes. Medmindre du konfigurerer persistens, holder RabbitMQ dine beskeder i hukommelsen. Det betyder, at de går tabt, når tjenesten genstartes.  
- **Auto-delete**
    - Det er muligt at konfigurere en exchange eller kø til at slette sig selv, hvis der ikke længere er nogen tilkoblinger.  
    - For eksempel kan en kø automatisk blive fjernet, når den sidste consumer afbryder forbindelsen.  

<small>Kilde: [LinkedIn Learning: Learning RabbitMQ - Overview of RabbitMQ](https://www.linkedin.com/learning/learning-rabbitmq/overview-of-rabbitmq?resume=false&u=57075649)</small>