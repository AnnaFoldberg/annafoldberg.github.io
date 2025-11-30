---
title: "Namespaces"
categories: [kubernetes]
tags: [namespaces, containers, isolation]
lang: it
locale: it
nav_order: 59
ref: note-namespaces-container-vs-kubernetes
---

##### Container Namespaces

I container sono isolati dall’host tramite i namespaces. Un namespace limita quali processi il container può vedere, impedendogli di accedere ai processi dell’host o di altri container. Per il container sembra di essere l’unico sistema in esecuzione. L’host, invece, ha pieno accesso a tutti i processi, inclusi quelli in esecuzione nei container.

##### Namespaces in Kubernetes

I namespaces in Kubernetes vengono utilizzati per dividere logicamente le risorse all’interno di un cluster. Un namespace raggruppa gli oggetti correlati, separa ambienti come dev, test e prod e previene conflitti di nomi.

I workloads all’interno di un namespace possono accedere solo alle risorse presenti in quel namespace. Tuttavia, i pod possono comunque comunicare tra namespaces, a meno che ciò non venga limitato tramite NetworkPolicies. I namespaces supportano quindi sia la separazione che la comunicazione controllata.

Poiché i namespaces in Kubernetes sono divisioni logiche del cluster, non modificano l’isolamento effettivo del container a livello di host. Organizzano le risorse, ma non impediscono ai pod di vedere o contattare altri pod sulla rete sottostante. Pertanto, i namespaces non costituiscono una barriera di sicurezza a livello di nodo.

In Kubernetes, risorse come pod, services e deployments appartengono sempre a un namespace specifico. Se non viene specificato alcun namespace, le risorse vengono collocate automaticamente nel namespace default.

<small>Fonte: [Udemy Course: Certified Kubernetes Administrator](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/14296142#overview)</small>