---
title: "Trackunit: AnalysisSubscriptions - graph-gateway"
categories: [trackunit, microservices]
tags: [graphql, subscriptions, csharp, final]
lang: en
locale: en
nav_order: 44
ref: graph-gateway-analysis-subscriptions
---

This example shows how **graph-gateway** exposes analysis events to **trackunit-client** via GraphQL subscriptions.  
The two subscriptions, `onAnalysisStarted` and `onAnalysisCompleted`, are bound to the topics (`analysis/started` and `analysis/completed`) through **HotChocolate**’s event system.  

Both require the API scope `RequireApiScope`.

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