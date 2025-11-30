---
title: "Kubernetes Manifests"
categories: [kubernetes]
tags: [manifests, pods, replicasets, deployments]
lang: it
locale: it
nav_order: 57
ref: note-kubernetes-manifests
---

### Kubernetes Manifests

I manifest Kubernetes vengono utilizzati per descrivere lo stato desiderato dei workloads in un cluster. Sono file YAML dichiarativi che Kubernetes controlla e garantisce che corrispondano sempre allo stato effettivo. Tre delle risorse più fondamentali sono Pod, ReplicaSet e Deployment.

![Manifests](../../assets/images/notes/kubernetes-manifests/manifests.png)

**Pod**

Un Pod è l'unità distribuibile più piccola in Kubernetes e contiene uno o più container. Nelle architetture a microservizi si utilizza quasi sempre un rapporto 1:1, in cui un pod contiene un solo container, perché i microservizi sono mantenuti separati e possono essere scalati facilmente in modo indipendente.

Nell'illustrazione è mostrato un esempio di manifest Pod che descrive un container nginx. Nella sezione metadata vengono indicati nome, labels ed eventualmente il namespace. Nginx funziona qui come un normale web server in esecuzione nel container.

**ReplicaSet**

Un ReplicaSet ha il compito di garantire che un determinato numero di pod, definito nel campo replicas, sia sempre in esecuzione. Se un pod fallisce, il ReplicaSet ne crea automaticamente uno nuovo per mantenere il numero desiderato.

Il manifest centrale mostra un ReplicaSet con tre replicas, che corrispondono a tre pod identici.

**Deployment**

Un Deployment gestisce uno o più ReplicaSet e definisce la strategia per come i pod devono essere aggiornati. Il manifest a sinistra mostra un Deployment che descrive tre replicas di un pod nginx.

La differenza tra Deployment e ReplicaSet è che il Deployment include strategie di aggiornamento, mentre il ReplicaSet si limita a mantenere x pod in esecuzione.

In pratica si crea quasi sempre solo il manifest Deployment. Kubernetes crea automaticamente il ReplicaSet e i pod. È considerata una cattiva pratica creare pod direttamente, perché non vengono ricreati in caso di errore. Un ReplicaSet può ricrearli, ma senza i vantaggi di una strategia di aggiornamento.

**Strategia di aggiornamento**

Nel Deployment viene utilizzata la strategia RollingUpdate. Essa controlla come vengono distribuite le nuove versioni dei pod. Le impostazioni:

- `maxUnavailable: 1` significa che al massimo un pod può essere non disponibile durante un aggiornamento.
- `maxSurge: 1` significa che può essere creato temporaneamente un pod aggiuntivo.

Durante un aggiornamento possono quindi esserci brevemente quattro pod: uno vecchio in fase di terminazione, uno nuovo in fase di avvio e due pod esistenti completamente funzionanti. Questo garantisce un aggiornamento controllato e stabile senza downtime.