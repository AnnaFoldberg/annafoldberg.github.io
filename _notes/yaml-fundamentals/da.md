---
title: "Grundlæggende om YAML"
categories: [kubernetes]
tags: [yaml, gitops, infrastructure-as-code]
lang: da
locale: da
nav_order: 39
ref: note-yaml-fundamentals
---
Infrastructure as Code og GitOps er to tilgange, der ofte bruges sammen med Kubernetes. Med Kubernetes kræver opsætning af infrastruktur gennem kode og deployment af ændringer med en GitOps-workflow, at den ønskede tilstand af clusteret gemmes i filer, der spores i et versionsstyringssystem som Git. Det format, der oftest anvendes til disse filer, er YAML-manifester.

YAML er et dataserialiseringssprog, ofte brugt til konfigurationsfiler. Dataserialiseringssprog giver en standardiseret måde at repræsentere information på, så forskellige enheder, operativsystemer og programmeringssprog let kan dele data. Andre eksempler inkluderer JSON og XML. Brug af et serialiseringssprog gør data portable; YAML gør det muligt at pakke data sammen, flytte dem og pakke dem ud et andet sted uden ekstra trin fra udvikleren.

Navnet YAML står for “YAML Ain’t Markup Language.” Det er designet til at være menneskelæsbart i modsætning til bytekode eller assembler, som giver instruktioner til en CPU, men er svære at forstå for mennesker. Ved at lære nogle få regler for YAML-syntaks bliver det meget lettere at arbejde med Kubernetes-manifester.

##### Vigtige YAML-syntaksregler
- `---:` Tre bindestreger markerer begyndelsen på et dokument. Flere dokumenter kan eksistere i samme fil, hver startende med ---.  
- `#:` Et hash i starten af en linje markerer en kommentar.  
- `key: value:` YAML gemmer data som key-value pairs adskilt af et kolon.  
- `- item:` Sekvenser (en liste eller array af elementer) bruger en bindestreg efterfulgt af et element på sin egen linje.  
- **Nested maps:** Indrykkede key-value pairs skaber strukturerede samlinger, såsom jobs med årstal og titler.  

##### Best Practices
- **Filendelser:** Både .yaml og .yml er gyldige.  
- **Indrykning:** YAML er følsom over for indrykningsfejl. Brug en validator (f.eks. yamlchecker.com) til at finde fejl.  

<small>Kilde: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>