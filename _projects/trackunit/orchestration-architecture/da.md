---
title: "Trackunit: Analysis Orchestrators Rolle"
categories: [trackunit, microservices]
tags: [architecture, messaging, events, commands, final]
lang: da
locale: da
nav_order: 49
ref: orchestration-architecture
---

**svc-analysis-orchestrator** er designet til at være det centrale koordinationspunkt for hele analyseflowet i systemet. Selvom den nuværende implementation er forenklet af hensyn til tidlig udvikling, er arkitekturen forberedt til en skaleret version, hvor orchestratoren styrer analyseforløbet gennem eksplicitte commands og domænespecifikke events.

Formålet er at have én service, der ved præcist, hvad der skal ske i analysen, hvornår det skal ske, og hvornår hele processen er fuldført.

På den måde kan hver analyseservice holdes lille, specialiseret og uafhængig, mens orchestratoren styrer det samlede flow.

##### Fuld orkestrering i et skaleret setup

###### Commands

Orchestratoren udsender commands for at igangsætte de konkrete analysetrin:

- `analysis.recognition.request`: Initierer billedanalyse hos **svc-ai-vision-adapter**
- `analysis.comparison.request`: Igangsætter sammenligning af resultater
- `analysis.metadata.request`: Igangsætter metadataanalyse eller sortering

###### Events

De enkelte analysetjenester udsender events, når de har udført deres del af arbejdet:

- `analysis.started`: Orchestratoren har modtaget `tu.image.uploaded` og påbegyndt analysen.
- `recognition.completed`: Udsendes af **svc-ai-vision-adapter**. Consumes af:
  - **graph-gateway**, der udsender et tilsvarende GraphQL-event til **trackunit-client**.
  - **svc-analysis-orchestrator**, der logger status og tidspunkt og igangsætter næste analysetrin.
- `comparison.completed`: Udsendes af **comparison-service**. Consumes af:
  - **graph-gateway**, der udsender et GraphQL-event videre.
  - **svc-analysis-orchestrator**, der fortsætter workflowet.
- `metadata.sorted`: Udsendes af **metadata-service**. Consumes af:
  - **graph-gateway**, der udsender GraphQL-event.
  - **svc-analysis-orchestrator**, der logger status og udsender `analysis.completed`.
- `analysis.completed`: Udsendes af **svc-analysis-orchestrator** når analysen er færdig. Consumes af:
  - **graph-gateway**, der udsender GraphQL-event til **trackunit-client**.
  - **svc-machine-storage**, der gemmer maskindata.

For alle succes-events findes et tilsvarende `*.failed`-event. Disse failures bruges af **svc-analysis-orchestrator** og **graph-gateway** til at håndtere fejlscenarier og opdatere **trackunit-client**.

##### Fordele ved denne arkitektur

- Commands sendes point-to-point til én klart defineret handler.  
- Events publiceres bredt, så flere komponenter kan reagere.  
- **svc-analysis-orchestrator** styrer rækkefølgen, men udfører ikke selve analysearbejdet.  
- Analysetjenesterne er små, specialiserede og nemme at udvide.  
- Persistence håndteres udenfor orchestratoren, så workflowet fortsætter uden blokeringer.  
- **trackunit-client** modtager kun events, aldrig interne commands.  
- Arkitekturen gør det muligt at skalere både tjenester og analysetrin uafhængigt.