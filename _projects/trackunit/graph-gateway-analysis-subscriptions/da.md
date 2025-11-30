---
title: "Trackunit: AnalysisSubscriptions - graph-gateway"
categories: [trackunit, microservices]
tags: [graphql, subscriptions, csharp, final]
lang: da
locale: da
nav_order: 44
ref: graph-gateway-analysis-subscriptions
---

Dette eksempel viser, hvordan **graph-gateway** eksponerer analyse-events til **trackunit-client** via GraphQL-subscriptions.  
De to subscriptions, `onAnalysisStarted` og `onAnalysisCompleted`, er bundet til topics (`analysis/started` og `analysis/completed`) via **HotChocolate**’s eventsystem.  

Begge kræver API-scope `RequireApiScope`.

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