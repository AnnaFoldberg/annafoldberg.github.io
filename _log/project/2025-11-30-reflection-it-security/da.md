---
title: "2025-11-30: IT-sikkerheds Rolle i Projektet"
categories: [it-security, kubernetes, microservices, trackunit]
tags: [reflection, project, individual]
lang: da
locale: da
nav_order: 19
ref: log-2025-11-30-reflection-it-security
---

##### Sikkerhedstiltag projektet dækker

Projektet gav mulighed for at arbejde med flere centrale sikkerhedsmekanismer i et cloud-native setup. Den vigtigste del var adgangskontrollen, hvor jeg implementerede rate-limiting i Kong og OIDC-baseret autentifikation via oauth2-proxy og efterfølgende token-validering i graph-gateway. Det betød, at kun klienter med gyldige JWT-tokens kunne kalde API-gatewayen, og at uautoriserede forespørgsler eller misbrugskald blev stoppet tidligt i flowet.

Jeg arbejdede også med transportlagssikkerhed ved at sætte TLS-terminering op via en Ingress Controller med selvsignerede certifikater, så trafikken mellem klienten og systemets indgangspunkt blev krypteret.

Derudover opsatte jeg Network Policies, som begrænser trafikken mellem pods og giver fin-granuleret kontrol over, hvilke services der må tale sammen. Det reducerede angrebsfladen internt i clusteret ved kun at tillade nødvendig trafik og blokere alt andet.

Endelig brugte jeg Kubernetes Secrets til at håndtere følsomme oplysninger på en mere forsvarlig måde, så de ikke blev lagt direkte ind i deployments eller values-filer. Det gav en renere separation mellem konfiguration og secrets og en mere sikker måde at arbejde med credentials på.

##### Sikkerhedstiltag projektet ikke dækker

Der er også sikkerhedsaspekter, som projektet bevidst ikke indeholder. Jeg har eksempelvis ikke brugt et service mesh til mTLS mellem services. Selvom det giver stærkere identitetshåndtering og kryptering inde i selve clusteret, ville det have tilføjet en del ekstra kompleksitet, som ikke matchede projektets omfang eller læringsmål.

Jeg har heller ikke arbejdet med centraliseret logging og tracing. I et produktionsmiljø er det vigtigt for at opdage mistænkelig aktivitet og undersøge fejl, men her valgte jeg at fokusere på de grundlæggende mekanismer som adgangskontrol, TLS, Secrets og netværksbegrænsning.

##### Opsummering

Projektet dækker de mest relevante sikkerhedselementer for et system af denne størrelse, herunder kryptering mellem klient og cluster, autentifikation, rate-limiting, token-validering, Secrets-håndtering og begrænsning af netværkstrafik. De mere avancerede teknologier som service mesh og fuld observability er fravalgt for at holde fokus på de områder, der giver mest læringsværdi uden at introducere unødvendig kompleksitet.