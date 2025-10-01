---
title: "Kubernetes Security"
categories: [it-security, kubernetes]
tags: [security-context, snyk]
lang: da
locale: da
nav_order: 54
ref: note-kubernetes-security
---
Som ethvert internet-eksponeret system er Kubernetes-clusters mål for angreb. Almindelige mål inkluderer at stjæle data, overtage cluster-ressourcer til kryptovalutamining eller starte distributed denial-of-service (DDoS)-angreb. Selvom trusler udvikler sig, er der flere best practices, der kan anvendes med det samme for at forbedre sikkerheden i clusteret.

##### Security Context
Containere bør konfigureres med restriktive sikkerhedsindstillinger for at reducere risikoen, hvis en angriber får adgang. Centrale praksisser inkluderer:  
- Kør containere som non-root-brugere.  
- Deaktiver privilege escalation.  
- Drop alle Linux-capabilities, der ikke eksplicit er nødvendige.  

##### Read-Only Root Filesystem
Sæt containerens filesystem til read-only med `readOnlyRootFilesystem: true` for at forhindre angribere i at ændre filer eller installere software inde i containeren.

##### Scan med Sikkerhedsværktøjer
Brug værktøjer som **Snyk** til at scanne Kubernetes-manifester for misconfigurations. Scanninger fremhæver problemer såsom skrivbare root-filesystems, manglende bruger-kontrol og fravær af privilege-restriktioner. Regelmæssige scanninger hjælper med at håndhæve sikre konfigurationer.

##### Opdater Kubernetes Regelmæssigt
Hold Kubernetes-clusters opdateret, især når sikkerhedsrettelser frigives. Overvåg [CVE-databasen](https://cve.mitre.org/) for kendte Kubernetes-sårbarheder og anvend rettelser hurtigt.

##### Opsummering
Forbedring af Kubernetes-sikkerhed begynder med simple men effektive tiltag: konfigurer Pods med security contexts, håndhæv read-only filesystems, scan manifester med sikkerhedsværktøjer, og opdater Kubernetes regelmæssigt for at anvende sikkerhedsrettelser.

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>