---
title: "Access Scenarios"
categories: [microservices, it-security]
tags: [access-control, api-gateway]
lang: da
locale: da
nav_order: 20
ref: note-access-scenarios
---
Adgang til en microservice sker gennem en klient. I de fleste systemer rutes klientkald gennem API gatewayen, som centraliserer adgang og kontrol.

Klienter adskiller sig i, hvordan de håndterer legitimationsoplysninger. **Public clients**, såsom single-page applikationer, der kører i en browser, kan ikke holde legitimationsoplysninger hemmelige. **Confidential clients**, såsom server-side applikationer, kan beskytte dem i højere grad.

Ejerskab spiller også en rolle. **First-party clients** tilhører den samme organisation, der har bygget tjenesten, mens **third-party clients** er eksterne og kræver strengere sikkerhedskontroller, da deres sikkerhedsstatus ikke kan garanteres. Nogle organisationer anvender disse strengere standarder på begge typer.

Endelig kan klienter være **interne** eller **eksterne**. Interne applikationer kører inden for virksomhedens firewall, mens eksterne står over for det offentlige internet. Netværksplacering bør dog aldrig reducere sikkerhedskravene, da insidertrusler og lateral bevægelse stadig er mulige, når først perimetret er brudt.

<small> Kilde: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>