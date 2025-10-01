---
title: "Sikkerhed i Container-images"
categories: [microservices, it-security]
tags: [containers, image-security]
lang: da
locale: da
nav_order: 32
ref: note-container-image-security
---
Container-images bygges ud fra Dockerfiles, som definerer instruktionerne for at tilføje og konfigurere en microservice. Hver instruktion tilføjer et nyt lag til imaget, startende fra et base image, ofte leveret af betroede kilder som Microsoft, Red Hat eller officielle Linux-distributioner. Når et image er bygget, gemmes det i et registry og hentes for at køre som container.

Sikkerhed begynder med brug af officielle base images fra betroede repositories. Uverificerede images kan introducere ondsindet kode og skabe et supply chain-angreb, der kompromitterer produktionssystemer. Officielle images opdateres rutinemæssigt for at lukke sårbarheder, så det er kritisk at holde sig ajour.

**Best practices for image-vedligeholdelse:**

- Scan registries regelmæssigt for sårbarheder med automatiserede værktøjer som Snyk  
- Genopbyg images med opdaterede base images, når CVE’er opdages  
- Genudrul containere for at erstatte forældede instanser  
- Foretræk små base images for at reducere angrebsfladen og begrænse angribers muligheder  

Disse praksisser sikrer, at containeriserede microservices forbliver sikre og modstandsdygtige over for udviklende trusler.

<small> Kilde: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>