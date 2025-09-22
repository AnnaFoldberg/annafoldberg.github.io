---
title: "Sikring af East–West-trafik"
categories: [microservices, it-security]
tags: [api-security, jwt, tokens, access-control]
lang: da
locale: da
nav_order: 27
ref: note-east-west-traffic
---
Microservices følger single responsibility-princippet, hvilket gør sikker kommunikation mellem dem afgørende. East–west-trafik refererer til service-til-service kald inde i systemet og rejser spørgsmål om, hvordan brugeridentitet deles, og hvor adgangspolitikker håndhæves.

Et almindeligt anti-pattern er at bruge ét access token med et delt scope på tværs af services. Dette skaber tæt kobling, svækker least-privilege-princippet og risikerer at eksponere følsomme services direkte.

En strategi er, at en service selv anmoder om et access token via client credentials-grant, før den kalder en anden service. Dette adskiller scopes, men den efterfølgende service mister kendskab til slutbrugeren og må stole på kaldende part. Hvis denne kompromitteres, kan den handle på vegne af enhver bruger.

En stærkere tilgang er at videregive brugeridentitet sammen med access token, typisk gennem en JWT, der indeholder claims om slutbrugeren. Ved at verificere JWT-signaturen kan den efterfølgende service håndhæve sine egne adgangskontrolbeslutninger. For at beskytte brugerdata modtager klienter ofte kun et reference token; ved API-gatewayen udveksles dette til et struktureret token med claims, som internt sendes mellem services.

Disse teknikker gør det muligt for microservices at etablere bruger­kontekst uden delt sessionstilstand. Omhyggeligt design er nødvendigt for at forhindre, at scopes eller claims kobler services for tæt sammen, samtidig med at sikkert samarbejde bevares.

<small> Kilde: [LinkedIn Learning: Securing Microservices](https://www.linkedin.com/learning/microservices-security/securing-microservices?contextUrn=urn%3Ali%3AlyndaLearningPath%3A645bcd56498e6459e79b3c71&resume=false&u=57075649)</small>