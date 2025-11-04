---
title: "Commands vs Events"
categories: [trackunit, microservices]
tags: [messaging, rabbitmq, architecture, naming-conventions]
lang: da
locale: da
nav_order: 7
ref: commands-events
---

##### Commands
Commands er målrettede beskeder. Præcis én service skal håndtere hver besked. Går til den service, der ejer beslutningen/tilstanden.  
*Direct exchange type* er standard for commands. Det sikrer, at kun én forbruger håndterer kommandoen. Den bruger simpel routing med en routing key, der svarer til kønavnet.

##### Events
Events er broadcast-beskeder. Mange services kan abonnere. De udsendes fra en service, efter at den har ændret tilstand.  
*Fanout exchange type* er standard for events. Det sikrer, at alle abonnenter modtager alle beskeder, og det er både simpelt og effektivt.  
Alternativt kan *topic exchange type* bruges, hvis filtrerede abonnementer er nødvendige. Det gør det muligt for services at lytte til delmængder af events baseret på routing key-mønstre.

##### Navngivningskonventioner
- `ICommandPublisher` bruger **SendAsync** til commands for at understrege direkte, “gør dette”-semantik og stærkere leveringsforventninger.  
- `IEventPublisher` bruger **PublishAsync** til events for at understrege broadcast, “er allerede sket”-semantik.  
- Begge interfacenavne inkluderer **publisher**, fordi de altid publicerer til en broker (RabbitMQ).