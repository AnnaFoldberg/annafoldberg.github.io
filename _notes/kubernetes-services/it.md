---
title: "Services in Kubernetes"
categories: [kubernetes]
tags: [services, clusterip, nodeport, loadbalancer]
lang: it
locale: it
nav_order: 60
ref: note-kubernetes-services
---

Un Service in Kubernetes è un punto di accesso stabile che permette di raggiungere uno o più Pod tramite un IP e una porta fissi. Poiché i Pod vengono spesso riavviati, spostati o scalati, un Service garantisce che i client non debbano tenere traccia dei loro indirizzi IP variabili.

Un Service raggruppa un insieme di Pod, tipicamente tramite una selector, ed espone tali Pod come un unico endpoint logico. Questo significa che le applicazioni dentro o fuori dal cluster possono sempre raggiungere lo stesso indirizzo, indipendentemente da quali Pod siano attualmente in esecuzione.

Kubernetes fornisce tre tipi principali di Service, ClusterIP, NodePort e LoadBalancer, che determinano se il servizio è solo interno, raggiungibile tramite gli IP dei nodi o esposto attraverso un load balancer esterno.

##### ClusterIP (default)  
Espone il servizio internamente al cluster tramite un cluster IP interno. Utilizzato per la comunicazione Pod-to-Pod e non offre accesso esterno. Kubernetes crea questo tipo di servizio se non viene specificato altro.

##### NodePort  
Rende il servizio accessibile dall’esterno del cluster aprendo una porta statica (NodePort) su ogni nodo. La porta è compresa tra 30000–32767 e può essere raggiunta, ad esempio, tramite http://\<node-ip\>:30080.

Kubernetes crea anche un ClusterIP interno, allo stesso modo di un servizio di tipo ClusterIP, quindi il servizio funziona sia internamente che esternamente. Tutto il traffico inviato a nodeIP:nodePort viene automaticamente inoltrato al Service e poi ai Pod associati.

**Svantaggi di NodePort**  
Per accedere al servizio dall’esterno, il client deve conoscere gli indirizzi IP dei nodi, ed è il client a scegliere quale nodo contattare. Se il client contatta un nodo che successivamente smette di funzionare, il servizio apparirà irraggiungibile, anche se la NodePort rimane attiva sugli altri nodi. Per questo motivo, NodePort è usato principalmente per sviluppo, test, ambienti kind e semplici proof-of-concept in cui l’alta disponibilità non è fondamentale.

##### LoadBalancer  
Espone il servizio all’esterno tramite un load balancer esterno. Distribuisce automaticamente il traffico ai nodi e ai Pod che eseguono il servizio ed è comunemente usato in produzione per la sua stabilità e semplicità di accesso.

Kubernetes non fornisce un load balancer integrato. Deve quindi essere aggiunto manualmente oppure fornito da un cloud provider.

Nei cloud environment, il load balancer del provider gestisce automaticamente il routing verso i nodi appropriati. In kind, il tipo LoadBalancer funziona solo se viene installato uno strumento che simula un load balancer esterno (ad es. cloud-provider-kind).

<small>Fonte: [Kubernetes Documentation](https://kubernetes.io/docs/concepts/services-networking/service/)</small>