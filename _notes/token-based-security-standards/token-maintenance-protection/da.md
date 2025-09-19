---
title: "Vedligeholdelse og Beskyttelse af Tokens"
categories: [microservices, it-security]
tags: [tokens, oauth2]
lang: da
locale: da
nav_order: 25
ref: note-token-maintenance
---
I det bedste scenarie er et token gyldigt indtil dets udløbsdato, hvorefter det ikke længere kan bruges til at tilgå en microservice. Access tokens er typisk kortlivede, defineret af et *expires_in*-claim eller tilsvarende. Korte levetider reducerer risikoen, hvis et token kompromitteres. Når længere adgang er nødvendig, kan et refresh token udstedes for at opnå nye access tokens uden brugerens involvering.

Refresh tokens medfører egne risici, da de ligner passwords, hvis de bliver stjålet. For at modvirke dette kan de roteres, så et nyt refresh token udstedes hver gang et bruges.

I det værste scenarie skal kompromitterede tokens tilbagekaldes. Dette kræver, at tokens gemmes, og ofte skal alle tokens, der er udstedt til en bruger, ugyldiggøres. Hvis tokens ikke er persisteret, er tilbagekaldelse kun mulig ved at vente på udløb. OAuth definerer en token revocation-specifikation, der stiller et endpoint til rådighed for eksplicit at tilbagekalde tokens.

**Best Practices**

- Brug altid TLS til transport  
- Beskyt tokens som legitimationsoplysninger, inkl. client ID og secret  
- Angiv udløbsdatoer, hold dem korte, og brug refresh tokens om nødvendigt  
- Indsæt ikke følsomme oplysninger i token payloads  

<small> Kilde: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>