---
title: "Grundlæggende Sikkerhedsprincipper"
categories: [microservices, it-security]
tags: [cia-triad, authentication, authorization, attack-surface]
lang: da
locale: da
nav_order: 16
ref: note-security-fundamentals
---
**Fortrolighed:** Kontrol af adgang til data for at forhindre uautoriseret videregivelse  

**Integritet:** Sikre at data ikke ændres og kan stoles på  

**Tilgængelighed:** Autoriserede brugere kan få adgang til data, når det er nødvendigt  

For at opnå disse mål vælges og anvendes sikkerhedsstrategier og kontroller i en systems arkitektur i det omfang, de er nødvendige.

![CIA triad](../../assets/images/notes/security-fundamentals.png)

**Adgangskontrol**

1. Autentificering: Etablér identitet  
2. Autorisation: Bekræft adgangsrettigheder  
3. Ressourceadgang  

**Autentificering**

- Identitetsverificering af parter, der tilgår et system  
- Menneskelig eller ikke-menneskelig (system)  
- Legitimation eller tokens beviser identitet  
- Højsensitive systemer kan anvende multifaktorautentificering (MFA)  

**Autorisation**

- Sikrer autoriseret adgang og brug af ressourcer  
- Identifikation og håndhævelse af privilegier  
- Kan delegeres til tredjepart  

**Troværdighed**

- Afgør i hvilket omfang noget anses for at være sandt.  

**Angrebsflade**

Omfatter alle de veje, der kan bruges til at få data ind i eller ud af en applikation. Et systems brugergrænseflade, åbne porte, API og database kan alle give muligheder for, at en angriber kan kompromittere systemet. Derfor betragtes de som en del af angrebsfladen. At reducere og forstærke angrebsfladen er en effektiv strategi til at styrke et systems sikkerhed.

<small> Kilde: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>