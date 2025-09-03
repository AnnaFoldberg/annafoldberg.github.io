---
title: "Containerizzazione"
categories: [kubernetes]
tags: [containers]
lang: it
locale: it
nav_order: 5
ref: note-containerization
---
C’è un processo in 3 fasi quando si lavora con i container e si vogliono creare o pubblicare container:
1. Manifesto – qualcosa che descrive il container stesso (in Docker un Dockerfile, in Cloud Foundry un manifest YAML)  
2. Creare l’immagine vera e propria (in Docker una Docker image, in Rocket un ACI – Application Container Image)  
3. Fare push su un registro: otteniamo così il container, che contiene tutti i runtime, le librerie e i binari necessari per eseguire un’applicazione  

![VMs vs Containers](../../assets/images/notes/containerization/vms-vs-containers.png)

L’applicazione gira sul seguente setup:
- Host OS  
- Motore di runtime (ad es. Docker engine – qualcosa che esegue i container)  
⇒ Questo consuma un certo insieme di risorse  

Questo approccio è molto più leggero delle VM. Non dovendo gestire un sistema operativo guest, abbiamo soltanto le librerie e l’applicazione stessa. Poiché non dobbiamo duplicare le dipendenze del sistema operativo né creare VM, utilizziamo meno risorse.  
⇒ Restano quindi ancora buone quantità di risorse disponibili  

Se i processi del container non utilizzano CPU o memoria, tutte le risorse condivise diventano accessibili agli altri container in esecuzione sullo stesso hardware.

<small> Fonte: [YouTube: Containerization Explained](https://www.youtube.com/watch?v=0qotVMX-J5s)</small>