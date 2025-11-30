---
title: "Nomi dei Pod in Kubernetes"
categories: [kubernetes]
tags: [pods, naming, replicasets, deployments]
lang: it
locale: it
nav_order: 58
ref: note-kubernetes-pod-names
---

Un tipico nome di un pod Kubernetes può apparire così: `myapp-7c9d7bc5b8-2xkpf`. È composto da tre parti, ciascuna con un significato specifico.

1. `myapp`: Il nome proveniente dall’oggetto Deployment. Tutti i pod creati da un deployment iniziano con questo prefisso.
2. `7c9d7bc5b8`: Hash del ReplicaSet. Si tratta di un hash generato da Kubernetes per identificare il ReplicaSet associato alla versione corrente del Deployment. Ogni volta che spec.template cambia, viene creato un nuovo ReplicaSet con un nuovo hash.
3. `2xkpf`: Pod ID. È un suffisso univoco generato da Kubernetes per garantire che due pod non condividano mai lo stesso nome. Identifica il singolo pod.

È quindi possibile avere pod come questi, appartenenti allo stesso Deployment e ReplicaSet, ma ciascuno con il proprio Pod ID univoco:

```yaml
myapp-7c9d7bc5b8-2xkpf
myapp-7c9d7bc5b8-hlsbr
myapp-7c9d7bc5b8-rrlqt
```