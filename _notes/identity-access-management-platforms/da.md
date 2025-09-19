---
title: "Identity and Access Management Platforme"
categories: [microservices, it-security]
tags: [iam, access-control, tokens]
lang: da
locale: da
nav_order: 18
ref: note-iam-platforms
---
**IAM-platform funktioner:**

- **Autentificering:** Understøtter forskellige strategier til at verificere identiteten af slutbrugere og klienter  
- **Identitetsstyring:** Administration af identitetsattributter og adgangsrettigheder for forskellige parter, inkl. slutbrugere og klienter  
- **Standardimplementeringer:** Leverer implementeringer af bredt anvendte standarder til adgangskontrol, identitetsverifikation og klientregistrering  
- **Token-håndtering:** Giver funktioner til at udstede, tilbagekalde og inspicere tokens i forskellige formater.  

![IAM Platform Funktioner](../../../assets/images/notes/iam-platforms/iam-capabilities.png)

Førende platforme integrerer med forskellige identitetslagre for at autentificere brugere. Disse lagre indeholder legitimationsoplysninger og rettigheder brugt til adgangskontrol, fra lokale databaser til virksomhedsapps eller sociale logins. For følsomme tilfælde kan multifaktorautentificering tilføje en ekstra faktor, såsom en engangskode eller biometrik.  

![IAM og Identitetslagre](../../../assets/images/notes/iam-platforms/iam-identity-stores.png)

Disse løsninger er også vigtige kilder til implementeringer af sikkerhedsstandarder og -protokoller, som bruges af microservices, såsom OAuth2, OpenID Connect og JSON Web Token.  

![IAM Standarder](../../../assets/images/notes/iam-platforms/iam-standards.png)

Billedet nedenfor illustrerer **token-services i en IAM-platform** og viser, hvordan tokens udstedes, fornyes, udløber, tilbagekaldes, verificeres, lagres og formateres for at understøtte autentificering og adgangskontrol.  

![IAM Token Services](../../../assets/images/notes/iam-platforms/iam-token-services.png)

<small> Kilde: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>