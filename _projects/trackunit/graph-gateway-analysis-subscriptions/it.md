---
title: "Trackunit: AnalysisSubscriptions - graph-gateway"
categories: [trackunit, microservices]
tags: [graphql, subscriptions, csharp, final]
lang: it
locale: it
nav_order: 44
ref: graph-gateway-analysis-subscriptions
---

Questo esempio mostra come **graph-gateway** espone gli eventi di analisi al **trackunit-client** tramite subscription GraphQL.  
Le due subscription, `onAnalysisStarted` e `onAnalysisCompleted`, sono collegate ai topic (`analysis/started` e `analysis/completed`) tramite il sistema di eventi di **HotChocolate**.  

Entrambe richiedono lo scope API `RequireApiScope`.

```csharp
namespace GraphGateway.GraphQL.Subscriptions;

[ExtendObjectType("Subscription")]
public class AnalysisSubscriptions
{
    [Authorize(Policy = "RequireApiScope")]
    [Subscribe]
    [Topic("analysis/started")]
    public AnalysisStarted OnAnalysisStarted([EventMessage] AnalysisStarted evt) => evt;

    [Authorize(Policy = "RequireApiScope")]
    [Subscribe]
    [Topic("analysis/completed")]
    public AnalysisCompleted OnAnalysisCompleted([EventMessage] AnalysisCompleted evt) => evt;
}
```