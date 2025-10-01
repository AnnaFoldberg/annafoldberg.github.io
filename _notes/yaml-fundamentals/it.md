---
title: "Fondamenti di YAML"
categories: [kubernetes]
tags: [yaml, gitops, infrastructure-as-code]
lang: it
locale: it
nav_order: 39
ref: note-yaml-fundamentals
---
Infrastructure as Code e GitOps sono due approcci spesso associati a Kubernetes. Con Kubernetes, la configurazione dell’infrastruttura tramite codice e il deployment delle modifiche con un flusso GitOps richiedono di salvare lo stato desiderato del cluster in file tracciati in un sistema di controllo versione come Git. Il formato più utilizzato per questi file è costituito dai manifest YAML.

YAML è un linguaggio di serializzazione dei dati, spesso utilizzato per i file di configurazione. I linguaggi di serializzazione dei dati forniscono un modo standard per rappresentare le informazioni in modo che diversi dispositivi, sistemi operativi e linguaggi di programmazione possano condividere facilmente i dati. Altri esempi sono JSON e XML. Utilizzare un linguaggio di serializzazione rende i dati portabili; YAML consente di impacchettare i dati, spostarli altrove e decomprimerli senza passaggi aggiuntivi da parte dello sviluppatore.

Il nome YAML significa “YAML Ain’t Markup Language.” È progettato per essere leggibile dalle persone, a differenza del bytecode o del linguaggio assembly, che forniscono istruzioni a una CPU ma sono difficili da comprendere per gli esseri umani. Imparare alcune regole di sintassi YAML rende molto più semplice lavorare con i manifest Kubernetes.

##### Elementi essenziali della sintassi YAML
- `---:` Tre trattini all’inizio di una riga indicano l’inizio di un documento. Più documenti possono esistere in un singolo file, ognuno dei quali inizia con ---.  
- `#:` Un hash all’inizio di una riga denota un commento.  
- `key: value:` YAML memorizza i dati come key-value pairs separate da due punti.  
- `- item:` Le sequenze (una lista o un array di elementi) usano un trattino seguito da un elemento su una propria riga.  
- **Nested maps:** Key-value pairs indentate creano collezioni strutturate, come lavori con anni e titoli.  

##### Best Practices
- **Estensioni file:** Sia .yaml che .yml sono validi.  
- **Indentazione:** YAML è sensibile agli errori di indentazione. Utilizzare un validatore (ad es. yamlchecker.com) per controllare eventuali errori.  

<small>Fonte: [LinkedIn Learning: Learning Kubernetes](https://www.linkedin.com/learning/learning-kubernetes-16086900)</small>