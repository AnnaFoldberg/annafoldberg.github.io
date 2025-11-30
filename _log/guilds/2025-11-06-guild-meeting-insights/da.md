---
title: "2025-11-06: Indsigter fra Guildmøde"
categories: [microservices, it-security]
tags: [guild, individual, reflection]
lang: da
locale: da
nav_order: 16
ref: log-2025-11-06-guild-meeting-insights
---

##### Microservices og Kubernetes

Det blev understreget, at eksamen handler om, hvad jeg personligt har lært om microservices og kubernetes, både teoretisk og praktisk. Projektet er kun et bevis på min læring, ikke et mål i sig selv. Jeg skal derfor forklare mine egne arkitekturvalg, mine implementeringer og hvordan jeg har tilegnet mig forståelsen, f.eks. gennem videoer og eksperimenter.

Frem for at gennemgå hvilke microservices projektet indeholder, giver det mere mening at forklare, hvordan microservices fungerer i teorien, og hvordan jeg selv har implementeret dem i praksis. Det blev understreget, at det centrale er mine egne arkitekturvalg, mine egne implementeringer og min forståelse af principperne, ikke en detaljeret gennemgang af andres kode eller services. Ligeledes behøver de andres services ikke at deployes i Kubernetes.

##### IT-sikkerhed

**CIA-modellen som ramme**  
Thomas anbefalede at strukturere IT-sikkerhed ud fra CIA-triaden: Confidentiality, Integrity og Availability. Modellen bruges til at vurdere, hvor sikkerhedsindsatsen giver mest mening i forhold til de data, systemet håndterer. Et helt sikkert system er umuligt, og sikkerhed er derfor en balance og en prioritering.

**Web- og API-sikkerhed**  
Vi gennemgik, hvorfor websikkerhed og API-sikkerhed er tæt beslægtet: Begge bygger på HTTP-protokollen (typisk port 80/443) og har mange af de samme angrebsflader og modforanstaltninger. Internettet er den underliggende infrastruktur, mens web blot er den del af trafikken, der bruger HTTP-protokollen.

**Databasesikkerhed**  
Databasesikkerhed knyttes til de samme tre søjler:

- **Confidentiality:** Adgangskontrol, mindst mulige rettigheder, kryptering af data i hvile og transit  
- **Integrity:** Logging, audit trails og mekanismer der sikrer, at data ikke kan ændres ubemærket  
- **Availability:** Firewall-opsætning, skjulte porte, sikring mod brute-force, backups og recovery-procedurer  

Vi vurderede ud fra CIA-triaden, hvilke områder følgende sikkerhedsmekanismer understøtter:

- Adgangskontrol og autentificering → **Confidentiality**  
- Regelmæssige sikkerhedsopdateringer → primært **Confidentiality**, men kan understøtte alle tre  
- Netværkssikkerhed og firewalls → **Availability** og **Integrity**  
- Overvågning og logging → **Integrity**  

**Websikkerhed**  
Vi gennemgik, hvordan de samme principper også bruges i webapplikationer. Trusler som XSS og SQL injection er eksempler på angreb, der udnytter HTTP som indgang.

Her vurderede vi igen ud fra CIA-triaden:

- Hvem må tilgå API’et? → **Confidentiality**  
- Kan vi stole på de data, vi modtager? → **Integrity**  
- Hvordan sikrer vi oppetid? Skal vi have en load balancer eller en recovery-plan? → **Availability**  

**Opsummering**  
Når man vurderer sikkerheden i et system, tager man udgangspunkt i domænet og dokumentationen og vurderer, hvor risikoen ligger og hvilke tiltag, der giver mening i praksis. Vi kan ikke få alle teknologier i spil, så det er vigtigt at vælge med omhu.

##### Forberedelse til næste guildmøder  
**Microservices og Kubernetes (20/11):** Vi skal tale om, hvordan vi vil præsentere vores projekt og vores faglige pointer til eksamen.  
**IT-sikkerhed:** Hvis vi ønsker feedback på læringsmål, kan de sendes et par dage inden næste guildmøde.