---
title: "Alta Disponibilità in Kubernetes"
categories: [kubernetes]
tags: [high-availability, pods, nodes, scaling]
lang: it
locale: it
nav_order: 66
ref: note-high-availability
---

Kubernetes è un orchestratore di container che gestisce il deployment e l’operatività di molti container attraverso un cluster.

##### Pod

Quando Kubernetes esegue un’applicazione, lo fa creando dei pod.

Un pod è un’istanza in esecuzione di un’applicazione. Possono esistere più pod della stessa applicazione per garantire alta disponibilità.

##### Nodi

I pod vengono eseguiti sui nodi. Un nodo è una macchina nel cluster, ad esempio un server virtuale. Un cluster è composto da più nodi. Un singolo nodo può:

- Eseguire pod da molte applicazioni diverse  
- Eseguire più pod della stessa applicazione  

Kubernetes decide dinamicamente dove posizionare i pod.

##### Alta disponibilità

Kubernetes garantisce alta disponibilità distribuendo i pod su più nodi.

Questo significa:

- Se un singolo pod fallisce → Kubernetes lo riavvia.  
- Se un intero nodo fallisce → tutti i pod su quel nodo scompaiono.  
- I pod sugli altri nodi continuano a funzionare normalmente.  
- Kubernetes avvia automaticamente nuovi pod sui nodi ancora attivi.  

In questo modo l’applicazione continua a funzionare anche se un’intera macchina del cluster va offline.

##### Scalabilità e gestione

Kubernetes distribuisce automaticamente il traffico tra i pod e può avviarne rapidamente altri quando il carico aumenta.  
È possibile aggiungere o rimuovere worker nodes senza interrompere l’applicazione.  
Il tutto è gestito tramite file di configurazione dichiarativi (manifests), che rendono l’amministrazione prevedibile e automatizzabile.

<small>Fonte: [Udemy: Learn Kubernetes](https://www.udemy.com/course/learn-kubernetes/)</small>