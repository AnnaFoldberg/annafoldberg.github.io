---
title: "Trackunit Prototype: Commands vs Events"
categories: [trackunit, microservices]
tags: [rabbitmq, architecture, naming-conventions]
lang: en
locale: en
nav_order: 7
ref: commands-events
---

##### Commands
Commands are targeted messages. Exactly one service should handle each message. Go to the service that owns the decision/state.  
*Direct exchange type* is default for commands. This guarantees one consumer handles the command. It uses simple routing with a routing key equal to the queue name.

##### Events
Events are broadcast messages. Many services may subscribe. They originate from a service after it changes state.  
*Fanout exchange type* is default for events. This ensures every subscriber gets all messages and is simple and efficient.  
Alternatively, *topic exchange type* can be used if filtered subscriptions are needed. This lets services listen to subsets of events based on routing key patterns.

##### Naming Conventions in Project
- `ICommandPublisher` uses **SendAsync** for commands to emphasize addressed, do-this semantics and stronger delivery expectations.  
- `IEventPublisher` uses **PublishAsync** for events to emphasize broadcast, happened-already semantics.  
- Both interface names include **publisher** because they always publish to a broker (RabbitMQ).