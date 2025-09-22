---
title: "Sicurezza del Container Runtime"
categories: [microservices, it-security]
tags: [containers, runtime-security]
lang: it
locale: it
nav_order: 31
ref: note-container-runtime-security
---
Per ottenere i pieni benefici di un’architettura a microservizi serve un ambiente di esecuzione flessibile che permetta distribuzioni rapide ma stabili. Per questo la maggior parte degli sviluppatori sceglie di distribuire microservizi all’interno di container.

I container hanno una propria superficie di attacco ed espongono nuovi vettori di attacco. Quando si eseguono microservizi in container, è possibile applicare misure di sicurezza per rafforzare vari componenti dell’architettura, incluso l’host e il container runtime.

**Sicurezza dell’host:**

- Limitare l’accesso (se si usa Kubernetes, questo numero dovrebbe essere ancora più basso, poiché la necessità di accedere all’host è ridotta)  
- Usare una distribuzione progettata per carichi di lavoro containerizzati (ad es. Red Hat Fedora CoreOS)  
  - Queste distribuzioni riducono la superficie di attacco dell’host rimuovendo le applicazioni non essenziali, diminuendo così le vulnerabilità presenti nel sistema operativo.  
- Applicare patch e hardening come per altre macchine virtuali  

**Configurazioni sicure del container runtime**

- **Least privilege:** Configurare i container per funzionare con il minor numero possibile di privilegi. Un approccio efficace è rimuovere inizialmente tutte le funzionalità del kernel e poi aggiungere solo quelle necessarie.  
- **Privilege escalation (flag no-new-privileges):** Impedisce ai container di aumentare i privilegi negandolo esplicitamente nella configurazione.  
- **Modalità privilegiata (flag privileged):** Non eseguire mai container con `--privileged`. Aumenta notevolmente il rischio di evasione dal container.  
- **Impostare un utente:** Configurare il container per funzionare come utente non privilegiato per prevenire attacchi di escalation.  
  - Impostare l’utente da utilizzare all’avvio dell’immagine, tramite configurazione o al lancio del container.  
  - Se non viene impostato alcun utente, il container verrà eseguito come root per impostazione predefinita, e root nel container equivale a root sull’host. Se un attaccante evade dal container, avrà accesso completo all’host sottostante.  
- **Read only:** Configurare i container con file system di sola lettura e montare i volumi come read-only.  

**Docker Bench:** Analizza i container in esecuzione e segnala configurazioni che deviano dagli standard di sicurezza del settore.  

- Script per verificare le best practices comuni nei container distribuiti.  
- Test in conformità con il CIS Docker Benchmark.  
- Open source e facile da eseguire.  
- [Docker Bench Security](https://github.com/docker/docker-bench-security)  

**Best practices:**

- Automatizzare il rilevamento delle configurazioni che deviano dalle pratiche sicure  
- Applicare configurazioni sicure nei pipeline CI/CD  
- Stabilire template per configurazioni sicure degli orchestratori di container  

<small> Fonte: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>