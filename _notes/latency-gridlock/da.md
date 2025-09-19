---
title: "Latency og Gridlock"
categories: [microservices]
tags: [architecture, reliability]
lang: da
locale: da
nav_order: 15
ref: note-latency-gridlock
---
Hvert servicekald introducerer en vis latency. For et enkelt kald er dette normalt ubetydeligt, men når anmodningsstier bliver mere komplekse, kan den samlede latency blive mærkbar for systemet.

Hvis det ikke håndteres, kan latency akkumulere og føre til gridlock, hvor dele af systemet ikke kan fortsætte. Gridlock kan også opstå, når cirkulære afhængigheder introduceres, for eksempel når en service indirekte kalder sig selv gennem andre services.

For at afbøde dette er det vigtigt at implementere timeout-logik på tværs af systemet, så latency eller cirkulære kald ikke forplanter sig uendeligt.

**Circuit breaker**

En circuit breaker er et almindeligt mønster til at håndtere latency og gridlock. I dette mønster, når timeouts forekommer, åbnes kredsløbet og en fallback-adfærd udføres i stedet for gentagne kald til den berørte service. Dette gør det muligt for systemet at fortsætte med begrænset funktionalitet i stedet for at fejle helt. Når de berørte services er gendannet, lukkes kredsløbet, og systemet genoptager normal drift.

<small> Kilde: [LinkedIn Learning: Microservices Foundations](https://www.linkedin.com/learning/microservices-foundations-23469069?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&u=57075649)</small>