---
title: "2025-10-23: Indsigter fra Guildmøde"
categories: [microservices, it-security]
tags: [guild, individual, reflection]
lang: da
locale: da
nav_order: 11
ref: log-2025-10-23-guild-meeting-insights
---
##### Microservices og Kubernetes  
**Logging og sikkerhed i microservices**  
Vi vendte, hvor essentielt logging egentlig er, og hvor meget det bør fylde i projektet. Jeg vælger at holde det på et vidensniveau, da det er vigtigt at forstå, hvordan logging og tracing bruges til at skabe overblik og spore fejl i et distribueret system, men jeg vurderer, at det ikke er nødvendigt at implementere det fuldt ud i denne omgang. I stedet fokuserer jeg på de mere konkrete sikkerhedsaspekter som JWT, OAuth og service meshes, da de spiller en mere central rolle i systemets overordnede sikkerhedsarkitektur.

**Messaging**
I forhold til messaging blev vi enige om, at det er vigtigt at have en vis dybde her, da microservices sjældent giver mening uden en form for kommunikationsmekanisme. Jeg bruger RabbitMQ til min del af systemet, mens andre medlemmer af projektgruppen kan vælge andre teknologier. Det vigtigste er, at jeg kan argumentere for mit valg.

**Perspektiv**
Vi kom desuden ind på, at det er relevant at overveje, om en microservices-arkitektur overhovedet er hensigtsmæssig i forhold til projektets størrelse. Mange af de komplekse elementer giver først for alvor værdi i større skala, hvor der er mange uafhængige services.

**Spikes**
Vi talte om at bruge små spikes til at afprøve nye teknologier i afgrænsede forsøg. Det giver mulighed for at tilegne sig praktisk erfaring og teste idéer i mindre skala, inden de eventuelt implementeres i det samlede projekt.