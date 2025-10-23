---
title: "2025-10-23: Indsigter fra Guildmøde"
categories: [microservices, it-security]
tags: [guild, individual, reflection]
lang: da
locale: da
nav_order: 13
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

##### IT-sikkerhed  
**Afgrænsning**  
Det blev afklaret, at fokus for IT-sikkerhed ligger inden for vores egen afgrænsning, og at der ikke forventes dækning ud over læringsmålene og indholdet i vores porteføljer.  

**HashiCorp Vault**  
Vi vendte, at HashiCorp (Vault) er nemt at sætte op sammen med Kubernetes, men at det i produktion bør køre på en separat server eller i en virtuel maskine fremfor som en almindelig container, da det kræver direkte systemadgang og stabil lagring.
Til demonstration eller prototype kan det dog fint køre i en container.  
HashiCorp understøtter tildeling af roller og tokens, der automatisk udløber eller deaktiveres, når roller lukkes. Det udsteder kun tokens til brugere med behov for adgang, og via audit-loggen kan man se, hvem der har tilgået hvilke ressourcer.  

**Forslag til Kubernetes kursus**  
[Certified Kubernetes Administrator (CKA) – LinkedIn Learning](https://www.linkedin.com/learning/certified-kubernetes-administrator-cka-cert-prep-25818035/lesson-11-lab-solution-setting-up-storage?u=57075649)