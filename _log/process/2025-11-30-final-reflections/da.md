---
title: "2025-11-30: Final Reflections"
categories: [microservices, kubernetes, it-security]
tags: [individual, reflection, process, project]
lang: da
locale: da
nav_order: 20
ref: log-2025-11-30-final-reflections
---

##### Indledende læringsmål

**Microservices og Kubernetes**  
I starten af semestret satte jeg mig som mål at opnå en dybere forståelse af, hvordan microservices og Kubernetes kan bruges i praksis til at bygge skalerbare og fleksible systemer. Jeg ønskede at kunne opbygge en arkitektur, hvor komponenter er velafgrænsede, løst koblede og kan udvikles og driftes uafhængigt. Kubernetes skulle samtidig bruges til at sikre stabilitet, skalerbarhed og driftssikkerhed.

**IT-sikkerhed**  
Jeg ønskede at arbejde med sikkerhed som en integreret del af arkitekturen. Målet var at kunne identificere væsentlige sikkerhedsrisici i et microservices-miljø med Kubernetes og vælge passende mekanismer til at beskytte data, kommunikation og infrastruktur.

##### Proces

Jeg startede semestret med en plan om først at indsamle og organisere information, da jeg ikke tidligere havde arbejdet med disse teknologier. Jeg fulgte læringsveje på LinkedIn Learning inden for microservices, Kubernetes og relateret IT-sikkerhed og fik her et overblik over centrale koncepter fra både et design-, brugs- og sikkerhedsperspektiv. Samtidig havde jeg behov for en hurtig forståelse af microservices og message brokers, så teamet kunne vælge en hensigtsmæssig arkitektur.

Da andre teammedlemmer også ønskede at stifte delvist bekendtskab med microservices-koncepter og messaging, fordelte vi ansvaret i systemet. Jeg tog ansvar for API-gatewayen og GraphQL-serveren samt den centrale analyseorkestrering. Det gav dem, der fokuserede mere på backend-emnet, mulighed for at arbejde med de mere logik-tunge services. Opdelingen var også naturlig, da systemet havde to hovedflows: upload af billede og analyse af billede.

Efter at have sat mig ind i microservices-principper, RabbitMQ og GraphQL lavede jeg en spike, **TeaApp**, for at afprøve microservices-arkitektur, autentificering via Microsoft Entra ID, Kong som API-gateway, GraphQL-operationer og publicering af events med RabbitMQ. Dette gav mig praktisk forståelse og et godt udgangspunkt for Trackunit-projektet.

Dernæst fulgte jeg dele af Kubernetes-læringsstien på LinkedIn Learning, men gik hurtigt over til Kubernetes’ officielle dokumentation, som jeg fandt let at omsætte direkte til praksis. Sideløbende brugte jeg Udemy-kurser til at forstå mere komplekse sammenhænge, særligt knyttet til sikkerhed og Kubernetes-komponenternes interne relationer.

Jeg planlagde også at dykke ned i OWASP API Security Top 10, OWASP Kubernetes Top 10 og Cyber Security Roadmap. Selvom jeg har orienteret mig i dem, valgte jeg ikke at gå i dybden, da jeg ville prioritere at fokusere på færre, men essentielle sikkerhedsmekanismer, som gik igen i kurser og artikler.

##### Trackunit Prototype i Docker

Jeg påbegyndte Trackunit-implementeringen med en prototype bestående udelukkende af de services, jeg selv arbejdede med: API-gatewayen (**kong** og **oauth2-proxy**), **graph-gateway** og **svc-analysis-orchestrator**.

**InfraCore** samlede alle services og infrastrukturkomponenter i ét docker-compose-miljø, så alt kunne startes samlet via et fælles docker-netværk. Jeg implementerede også en **ClientConsole**, så jeg kunne teste autentificering og GraphQL-flowet samt bekræfte, at events blev publiceret korrekt.

I denne fase ønskede teamet at flytte kommunikationen fra RabbitMQ til Kafka. Jeg eksperimenterede med det, men valgte at beholde RabbitMQ som message broker pga. manglende fundament for Kafka og ønsket om at fokusere på andre områder. Da teamet fortsatte med Kafka, besluttede jeg, at jeg senere i processen ville oprette en **messaging bridge**, som kunne konvertere Kafka-events til RabbitMQ-events. Den var oprindeligt tænkt to-vejs, men teamet besluttede, at **svc-ai-vision-adapter** skulle modtage `tu.image.uploaded` direkte fra **tu-ingestion-service**. Det betød, at **svc-analysis-orchestrator** mistede en central arkitekturrolle og i stedet blev reduceret til en notifier. Jeg valgte dog at beholde den, da den ville være vigtig i en mere skaleret version af systemet.

##### Overgang til Kubernetes

Herefter flyttede jeg systemet fra docker-compose til Kubernetes. Jeg valgte at køre alt lokalt i `kind`, da mit mål var at lære Kubernetes-drift og sikkerhedsmekanismer frem for at lægge vægt på en cloud-baseret løsning. Jeg udvalgte specifikke Kubernetes-ressourcer, der adresserer både drift og sikkerhed: deployments, services, secrets, namespaces og network policies.

Infrastrukturkomponenter implementerede jeg via Helm charts med values-filer for at sikre mere stabile, gennemarbejdede og sikkerhedsmæssigt forsvarlige konfigurationer. Dette gav mig bl.a. en **kong-proxy** LoadBalancer-service, som klienten kunne tilgå via `cloud-provider-kind`.

Til styring og overvågning af clusteret brugte jeg `k9s`, som gav et effektivt overblik over komponenter og logs.  

Derefter arbejdede jeg med nøgle- og certifikathåndtering for at etablere korrekt TLS-terminering mellem klienten og **kong-proxy**. I den forbindelse planlagde jeg at udvide opsætningen, så kong ikke kun fungerer som API Gateway, men også som Ingress Controller, hvor en Ingress-ressource definerer routing og TLS-konfiguration for ekstern trafik ind i clusteret. Denne opsætning sikrer, at al trafik uden for clusteret er krypteret under transport, og at certifikatet termineres sikkert på **kong-proxy**.  

Jeg valgte derimod ikke at indføre mTLS, hverken mellem klienten og kong-proxy eller internt i clusteret via et service mesh. Løsningen ville være overdimensioneret i forhold til projektets størrelse og læringsmål, men kan med fordel prioriteres i en senere iteration, hvor fokus er en mere komplet og dybdegående sikkerhedsmodel.  

Jeg havde oprindeligt planlagt at afprøve Kubernetes med **TeaApp** som et mellemtrin, men vurderede, at det ikke var nødvendigt, fordi det eksisterende docker-compose setup i Trackunit-projektet kunne overføres relativt enkelt til Kubernetes-manifester.  

##### Integration med Eksterne Services

Før integrationen med teamets øvrige microservices indførte jeg versionering af container-images. Det gjorde det muligt at rulle deployments tilbage, når nye versioner gav uforudsete problemer i kommunikationen mellem services.

Da jeg skulle integrere teamets microservices med mit cluster, valgte jeg ikke at deploye deres services i Kubernetes. Opsætning af ekstra komponenter som **Redpanda**, **MinIO** og en **Tailscale-sidecar**, oven i teamets microservices, ville introducere betydelig kompleksitet uden at tilføre tilsvarende læringsmæssig værdi på dette tidspunkt. Da Trackunit-projektet fungerer som et proof-of-concept, vurderede jeg, at det var vigtigere at fokusere på Kubernetes-drift og sikkerhed. I en senere iteration vil deres services naturligt kunne flyttes ind i clusteret.

Som del af integrationen implementerede jeg desuden **svc-messaging-bridge**, som sikrede broen mellem Kafka og RabbitMQ.

##### Opsummering

Gennem processen har jeg opnået de læringsmål, jeg satte for semestret. Det har resulteret i konkrete, individuelle læringsmål med kobling til Blooms taksonomi, som giver et klart overblik over både det teoretiske og praktiske niveau, jeg har arbejdet på.