---
title: "Namespaces"
categories: [kubernetes]
tags: [namespaces, containers, isolation]
lang: da
locale: da
nav_order: 59
ref: note-namespaces-container-vs-kubernetes
---

### Namespaces

**Container Namespaces**

Containere adskilles fra hosten ved hjælp af namespaces. Et namespace isolerer de processer, som containeren kan se, så den ikke har adgang til andre processer på hosten eller i andre containere. For containeren fremstår det, som om den er det eneste system. Hosten har derimod fuld adgang til alle processer, inklusive dem der kører i containere.

**Namespaces i Kubernetes**

Kubernetes-namespaces bruges til at opdele ressourcer logisk i et cluster. Et namespace afgrænser de objekter, der hører sammen, adskiller miljøer som dev, test og prod, og forebygger navnekonflikter.

Workloads i et namespace har kun adgang til ressourcer inden for det pågældende namespace. Pods kan dog stadig kommunikere på tværs af namespaces, medmindre dette begrænses ved hjælp af NetworkPolicies. Namespaces giver derfor mulighed for både adskillelse og kontrolleret kommunikation.

Da namespaces i Kubernetes fungerer som logiske opdelinger i clusteret, ændrer de ikke containerens faktiske isolation på hosten. De bruges til at organisere ressourcer, men de forhindrer ikke pods i at se eller kontakte hinanden på selve netværket. Derfor fungerer namespaces ikke som en sikkerhedsbarriere på node-niveau.

I Kubernetes tilhører ressourcer som pods, services og deployments altid ét specifikt namespace. Hvis man ikke angiver et namespace, placeres de automatisk i default-namespace.

<small>Kilde: https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn/lecture/14296142#overview</small>