---
title: "Sikkerhed i Container Runtime"
categories: [microservices, it-security]
tags: [containers, runtime-security]
lang: da
locale: da
nav_order: 31
ref: note-container-runtime-security
---
For at opnå de fulde fordele ved en microservices-arkitektur kræves et fleksibelt eksekveringsmiljø, der muliggør hurtige, men stabile udrulninger. Derfor vælger de fleste udviklere at køre microservices i containere.

Containere har deres egen angrebsflade og introducerer nye angrebsvektorer. Når microservices køres i containere, kan sikkerhedstiltag anvendes til at styrke forskellige komponenter i arkitekturen, herunder værten og container-runtime.

**Værts-sikkerhed:**

- Begræns adgang (hvis Kubernetes anvendes, bør dette tal være endnu lavere, da behovet for adgang til værten er reduceret)  
- Brug en distribution designet til container-workloads (f.eks. Red Hat Fedora CoreOS)  
  - Disse distributioner reducerer værtsens angrebsflade ved at fjerne ikke-essentielle applikationer. Dette mindsker antallet af sårbarheder i operativsystemet.  
- Patch og harden som ved andre virtuelle maskiner  

**Sikker konfiguration af container-runtime**

- **Mindste privilegier:** Konfigurer containere til at køre med færrest mulige privilegier. En effektiv tilgang er at fjerne alle kernel-capabilities først og derefter kun tilføje de nødvendige.  
- **Privilegium-eskalering (no-new-privileges-flag):** Forhindrer containere i at eskalere deres privilegier ved eksplicit at nægte det i konfigurationen.  
- **Privileged mode (privileged-flag):** Kør aldrig containere med `--privileged`. Det øger risikoen for container-udbrud markant.  
- **Sæt en bruger:** Konfigurer containeren til at køre som en ikke-privilegeret bruger for at forhindre eskaleringsangreb.  
  - Angiv den bruger, der skal anvendes, når image køres. Dette gøres enten i image-konfigurationen eller ved opstart af containeren.  
  - Hvis en bruger ikke angives, kører containeren som root som standard, og root i containeren er root på værten. Hvis en angriber slipper ud af containeren, har de fuld adgang til værten.  
- **Read only:** Konfigurer containere med skrivebeskyttede filsystemer og montér volumes som read-only.  

**Docker Bench:** Scanner kørende containere og rapporterer om konfigurationer, der afviger fra branchens sikkerhedsstandarder.  

- Script til at tjekke for gængse best practices i kørende containere.  
- Tester i henhold til CIS Docker Benchmark.  
- Open source og let at køre.  
- [Docker Bench Security](https://github.com/docker/docker-bench-security)  

**Best practices:**

- Automatiser detektion af konfigurationer, der afviger fra sikre praksisser  
- Håndhæv sikre konfigurationer i CI/CD  
- Etabler skabeloner for sikker konfiguration af container-orkestratorer  

<small> Kilde: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>