---
title: "Services i Kubernetes"
categories: [kubernetes]
tags: [services, clusterip, nodeport, loadbalancer]
lang: da
locale: da
nav_order: 60
ref: note-kubernetes-services
---

##### Services i Kubernetes

En Service i Kubernetes er et stabilt adgangspunkt, der gør det muligt at nå én eller flere pods via en fast IP og port. Da pods ofte genstartes, flyttes eller skaleres op og ned, sikrer en Service, at klienter ikke behøver at kende pods’ skiftende IP-adresser.

En Service grupperer et sæt pods, typisk med en selector, og eksponerer dem som ét logisk endpoint. Det betyder, at applikationer i eller udenfor clusteret altid kan nå den samme adresse, uanset hvilke pods der aktuelt kører.

Kubernetes har tre primære servicetyper, ClusterIP, NodePort og LoadBalancer, som bestemmer, om servicen kun er intern, tilgængelig via node-IP’er, eller eksponeret gennem en ekstern load balancer.

**ClusterIP (default)**  
Eksponerer servicen internt i clusteret via en intern cluster-IP. Bruges til pod-til-pod kommunikation og giver ingen ekstern adgang. Kubernetes opretter denne servicetype, hvis man ikke angiver andet.

**NodePort**  
Gør servicen tilgængelig udefra clusteret ved at åbne en statisk port (NodePort) på alle noder. Porten ligger i området 30000–32767 og kan tilgås direkte via f.eks. http://\<node-ip\>:30080.

Kubernetes opretter samtidig en intern ClusterIP, på samme måde som ved en service af typen ClusterIP, så servicen fungerer både internt og eksternt. Al trafik til nodeIP:nodePort videresendes automatisk til servicen og videre til de tilknyttede pods.

**Ulemper ved NodePort**  
For at tilgå servicen udefra skal man kende IP-adresserne på noderne, og det er op til klienten at vælge, hvilken node den vil kontakte. Hvis man rammer en node-IP og den node går ned, bliver servicen utilgængelig fra klientens perspektiv, selvom NodePort stadig er åben på de øvrige noder. Af den grund bruges NodePort primært til udvikling, test, kind-miljøer og simple proof-of-concepts, hvor høj tilgængelighed ikke er kritisk.

**LoadBalancer**  
Eksponerer servicen eksternt ved hjælp af en ekstern load balancer. Den fordeler automatisk trafikken til de noder og pods, der kører servicen, og bruges typisk i produktion, fordi den er stabil og nem at tilgå.

Kubernetes leverer ikke selv en load balancer. Derfor skal load balanceren tilføjes manuelt eller clusteret integreres med en cloud-udbyder.

I cloud-miljøer håndterer udbyderens load balancer al routing til de relevante noder. I kind fungerer LoadBalancer-typen kun, hvis man installerer et værktøj, der simulerer en ekstern load balancer (f.eks. cloud-provider-kind).

<small>Kilde: [Kubernetes Documentation](https://kubernetes.io/docs/concepts/services-networking/service/)</small>